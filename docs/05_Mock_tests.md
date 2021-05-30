# Mock Tests

In order to have a reliable CI pipeline, which is not dependent on the state of services,
a system is in place to automatically save the request made to a service and their responses.
These can then be used in the CI to reliably test the changes made to the Extractor and not have test failures due to 
API changes on the side of the service.

## Multiple downloader implementations
There are multiple implementations of the abstract class `Downloader`  
1. `DownloaderTestImpl` is used for running the test against the actual service.
2. `RecordingDownloader` is used the save the request and response to the disk, 
   thus creating the mock.
3. `MockDownloader` is used to answer request with the saved mocks.

### Configure usage
There are 2 ways to specify which downloader should be used

First one is passing the `-Ddownloader=<value>` argument from the command line, where `value` can be one of 
[DownloaderType](https://github.com/TeamNewPipe/NewPipeExtractor/blob/dev/extractor/src/test/java/org/schabi/newpipe/downloader/DownloaderType.java).
This is used in the CI pipeline like this: `./gradlew check --stacktrace -Ddownloader=MOCK` and also the main usecase.
Other than that it can also be used to mass generate mocks by specifying which package should be tested.
As example if one wanted to update all YouTube mocks:
`gradle clean test --tests 'org.schabi.newpipe.extractor.services.youtube.*' -Ddownloader=RECORDING` 

The second way is changing