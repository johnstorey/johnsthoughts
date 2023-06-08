---
title: "Vscode Run Failed Dotnet Tests"
date: 2023-06-07T15:09:40-05:00
draft: false
---

 Today I decided to look at the [C# Dev Kit for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit) released by Microsoft yesterday. I'm leaving my current position after this week, but we have a small issue that has stopped me and some co-workers from using Visual Studio Code for work. Instead we use Visual Studio or Rider. Since I use Visual Studio Code for everything else, this is a bit frustrating.

Unfortunately the problem persists in C# Dev Kit. Since it will probably still be an issue in my next position, using F#, I decided to try a quick fix.

# What is this "problem"?
Basically our integration test suite is growing every release (as they do.) Right now it's over 3000 tests. Some of them fail on the first run. With Visual Studio and Rider there is a nice button for "rerun failed tests". Usually we press it 2 or 3 times to get all the tests to pass.

(Let's not think about how that does not translate to CI pipelines at the moment).

There is no "dotnet test rerun-failed", so I took some time to look into it. As a result I have created some bash scripts and attached them to Visual Studio Code tasks. Now running all tests and just rerunning the failed tests is as simple as "SPC : :" with the [VSpaceCode Extension](https://marketplace.visualstudio.com/items?itemName=VSpaceCode.vspacecode). Command line is not a nice as a radiator in the GUI, but it does get the job done.

Here are the task configurations and the shell scripts. They can be used by you with some obvious customizations.

These are run on MacOS, so there may be some minor difference from grep and sed on Linux. If so I'll find out when running them in Dev Containers and come back and update this post.

 ## Task Configurations

```csharp
        {
            "label": "dotnet:runalltests",
            "command": "~/scripts/dotnet-test-run.sh",
            "type": "shell",
            "args": [
                "--runsettings",
                "/Users/johnstorey/Documents/projects/bios/parallel.dev.config.runsettings"
            ],
            "options": {
                "cwd": "${workspaceFolder}/IntegrationTests"
            },
            "group": {
                "kind": "test",
                "isDefault": true
            }
        },
        {
            "label": "dotnet:runfailedtests",
            "command": "~/scripts/dotnet-test-rerun-failed.sh",
            "type": "shell",
            "args": [
                "--runsettings",
                "/Users/johnstorey/Documents/projects/bios/parallel.dev.config.runsettings"
            ],
            "options": {
                "cwd": "${workspaceFolder}/IntegrationTests"j
            },
            "group": {
                "kind": "test",
                "isDefault": true
            }
        }
````

 ## Run All Tests

~/scripts/dotnet-test-run.sh

```shell
#!/bin/bash

# Parse command-line arguments
runsettings_file=""
output_directory="TestResults"

while [[ $# -gt 0 ]]; do
  key="$1"
  case $key in
    --runsettings)
      runsettings_file="$2"
      shift
      shift
      ;;
    --output-dir)
      output_directory="$2"
      shift
      shift
      ;;
    *)
      echo "Unknown option: $1"
      exit 1
      ;;
  esac
done

# Run dotnet test with the specified runsettings file (if provided)
if [[ -n $runsettings_file ]]; then
  dotnet test --settings "$runsettings_file" --logger "trx;LogFileName=$output_directory/TestResults.trx" --logger "console;verbosity=detailed"
else
  dotnet test --logger "trx;LogFileName=$output_directory/TestResults.trx" --logger "console;verbosity=detailed"
fi
```

## Only Run Failed Tests

~/scripts/dotnet-test-rerun-failed.sh

Notice I used the "--no-build" option here for "dotnet test". Each run of this overwrites the failed tests data, so you won't re-run passing tests if this is run multiple times.

```shell
#!/bin/bash

# Parse command-line arguments
output_directory="TestResults"

while [[ $# -gt 0 ]]; do
  key="$1"
  case $key in
    --runsettings)
      runsettings_file="$2"
      shift
      shift
      ;;
    --output-dir)
      output_directory="$2"
      shift
      shift
      ;;
    *)
      echo "Unknown option: $1"
      exit 1
      ;;
  esac
done

# Check if the test results directory exists
if [[ ! -d $output_directory ]]; then
  echo "Test results directory not found: $output_directory"
  exit 1
fi

# Check if the test results file exists
test_results_file="$output_directory/TestResults/TestResults.trx"
if [[ ! -f $test_results_file ]]; then
  echo "Test results file not found: $test_results_file"
  exit 1
fi

# Extract the failed test case names from the test results file

failed_tests=$(grep -E -o '<UnitTestResult[^>]+outcome="Failed"[^>]+>' "$test_results_file" | sed -E 's/.*testName="([^"]+)".*/\1/')

# Check if any failed tests were found

if [[ -z $failed_tests ]]; then
  echo "No failed tests found in the test results."
  exit 0
fi

# Read each line from the word list and append it to the array
# Needed for loop below.
tokens=()
while IFS= read -r line; do
  tokens+=("$line")
done <<< "$failed_tests"

# Create a filter expression
# Initialize the environment variable
filter_list=""

# Loop over the list of words and concatenate them with " | "
for word in "${tokens[@]}"; do

  # Trim the newline character
  word=$(echo "$word" | tr -d '\n')

  if [ -z "$filter_list" ]; then
    filter_list="$word"
  else
    filter_list="$filter_list | $word"
  fi
done

echo "($filter_list)"

# Rerun only the failed tests
if [[ -n $runsettings_file ]]; then
  # dotnet test --testlist "$test_list_file" --settings "$runsettings_file" --logger "console;verbosity=detailed"
  dotnet test --no-build --filter "($filter_list)" --settings "$runsettings_file" --logger "console;verbosity=detailed" --logger "trx;LogFileName=$output_directory/TestResults.trx"
else
  dotnet test --no-build --filter "($filter_list)" --settings "$output_directory/test.runsettings" --logger "console;verbosity=detailed" --logger "console;verbosity=detailed" --logger "trx;LogFileName=$output_directory/TestResults.trx"
fi
```

# Next Steps

It seems obvious to me how to put this into a pipeline that reruns failed tests N times before failing. That is something I'll pass on to my successor at work, though I know he has a similar solution in PowerShell that might do exactly the same thing.

Other than that, this meets my needs so I will probably not advance it further. But it's easy to see room for improvements or repurposing the logic into programs to

* Modify the list of tests to run to have the fully qualified name. The documentation of "dotnet test --filter" states that using just the test name, and I do, only works with MSTest. The fully qualified name runs with all tests. That may "just happen" with non MSTest frameworks.
* Add a third script to just list the failed tests. The code is in the script to rerun the failed tests already, and it gives some of the value of a dashboard.
* Make this a nice desktop app that is IDE independent. Parsing the TestResults/TestResults/TestResults.trx file will let you build the equivalent of the Visual Studio and Rider test runners easily enough.
* Make a Visual Studio Code extension using this solution pattern. I like that because many will want this in PowerShell and when it comes to scripting that I go blank. Ask Chat-GPT or GitHub CoPilot I guess. (I bet I could get them to rewrite this as a VS Code extension actually ... but no, my project plate is overflowing. Let me know if you do it.)