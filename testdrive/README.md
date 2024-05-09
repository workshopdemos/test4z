## Test4z Test Drive

Test4z challenges the notion that high-quality testing is inherently time-consuming by offering a solution that accelerates the development process.

In this overview, we'll explain the "what and how" of unit testing with Test4z. The brief hands-on exercise covers Test4z's:

* Code Coverage - learning what is and isn't tested
* Mocks - how to decouple your testing efforts from dependencies on a "live" environment
* Spies - how to validate code correctness, whether at the black or gray box level.

Ready to check out Test4z? Let's do it!

### Installation steps

Follow these steps to copy the exercise source files:
1. Clone the [workshopdemos/test4z](https://github.com/workshopdemos/test4z) repository.
2. In Visual Studio Code, open a new window on the `testdrive` folder (_File &gt; New Window_ followed by _File &gt; Open Folder..._)
3. If you have already installed Test4z, replace these files with your own local versions:

  - `test4z.project.config.json`
  - `test4z.user.config.json`
  - `.vscode/settings.json`
  
  If you have not installed and configured Test4z before, delete the above files from the cloned repository folder. Refer to these steps:
  
  1. [Install](https://techdocs.broadcom.com/us/en/ca-mainframe-software/devops/test4z/1-0/installing/install-test4z-command-line-interface.html) the Test4z command line interface
  2. Initialize the Visual Studio Code extension (`t4z install-vscode-extensions`)
  3. Configure the VS Code environment (`t4z init`).

  The Test4z installation command `t4z init` regenerates the configuration
  files above based on your responses (host name, user name, HLQ, etc.).

### References

* [Test4z Tutorial](https://github.com/workshopdemos/test4z/tree/main/workshop)
* [Test4z "code along"](https://www.youtube.com/watch?v=0hFXFf17kEI) video on YouTube (13 minutes).
* [Test4z Tutorial - Primer](https://github.com/workshopdemos/test4z/blob/main/docs/Test4z-Primer.pdf) (PDF).
* [Test4z homepage](https://mainframe.broadcom.com/test4z)
* [Test4z 1.0 documentation](https://techdocs.broadcom.com/us/en/ca-mainframe-software/devops/test4z/1-0.html)
