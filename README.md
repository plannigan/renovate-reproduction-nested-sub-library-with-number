# renovate-reproduction-nested-sub-library-with-number"

## Current behavior

This is a Gradle project that has a subproject that is nested under two folders. This project's name happens to include a number. As a result, the `settings.gradle.kts` file has the following line to specify the location of the subproject.

```
include("libs:foo:some-e2e-lib")
```

Note that the location looks like the normal dependency specifier.
Running Renovate against this project produces an update config containing:

```
...
{
 "packageFile": "settings.gradle.kts",
 "datasource": "maven",
 "deps": [
   {
     "depName": "libs:foo",
     "currentValue": "some-e2e-lib",
     "managerData": {
       "fileReplacePosition": 74,
       "packageFile": "settings.gradle.kts"
     },
     "fileReplacePosition": 74,
     "datasource": "maven",
     "registryUrls": ["https://repo.maven.apache.org/maven2"],
     "depType": "dependencies",
     "updates": [],
     "packageName": "libs:foo",
     "versioning": "gradle",
     "warnings": [
       {
         "topic": "libs:foo",
         "message": "Failed to look up maven package libs:foo"
       }
     ]
   }
 ]
},
...
```
However, since this is an `include()` entry in `settings.gradle.kts`, the value is not specifying a dependency. (If a digit character is not in the subproject's name, the warning is not raised.)

## Expected behavior

Subprojects included in the `settings.gradle.kts` file are not considered dependencies that could be updated. Which mean no warnings would be raised as a result of trying to find a package based on the subproject directory structure. 

## Link to the Renovate issue or Discussion

Put your link to the Renovate issue or Discussion here.
