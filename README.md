## Test4z Workshop - Tutorial and Exercise 

For many developers, testing is viewed as a necessary but time-consuming task. Why is that? It's because testing COBOL applications often involves numerous manual steps or setting up complex input data scenarios. Test4z challenges the notion that high-quality testing is inherently time-consuming by offering a solution that accelerates the development process.

For this exercise, you'll follow these top-level steps:

* Part 1: Write/verify unit test using the recording (mock API)
* Part 2: Add validation logic to the unit test (spy API)
* Part 3: Add pass/fail checks to the unit test (assert API)
* Part 4: Run final test (optionally make it fail).

Ready to write some code? Let's do it!

### Getting started

To complete this exercise, you have two options:

1. Detailed step-by-step instructions in the [Test4z Tutorial - Exercise](https://github.com/workshopdemos/test4z/blob/main/Test4z-Workshop.pdf) (PDF)
2. A short ["code along"](https://www.youtube.com/watch?v=0hFXFf17kEI) video on YouTube (13 minutes).

If you would like a short introduction to Test4z, check out the [Test4z Tutorial - Primer](https://github.com/workshopdemos/test4z/blob/main/Test4z-Primer.pdf) (PDF).

### Installation steps

Follow these steps to copy the exercise source files:
1. Clone the [workshopdemos/test4z](https://github.com/workshopdemos/test4z) repository.
2. In Visual Studio Code, open a new window on the `workshop` folder (_File &gt; New Window_ followed by _File &gt; Open Folder..._)
3. If you have already installed Test4z, replace these files with your own local versions:

  - `test4z.project.config.json`
  - `test4z.user.config.json`
  - `.vscode/settings.json`
  
  If you have not installed Test4z before, delete the above files from the cloned repository folder and run the Test4z installation command `t4z init`. It will prompt you for the installation parameters (host name, user name, HLQ, etc.).

Also see:

* [Test4z homepage](https://mainframe.broadcom.com/test4z)
* [Test4z 1.0 documentation](https://techdocs.broadcom.com/us/en/ca-mainframe-software/devops/test4z/1-0.html)
