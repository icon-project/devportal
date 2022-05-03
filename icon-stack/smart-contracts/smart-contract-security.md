# Smart contract security

### How to write more secure smart contract code <a href="#how-to-write-more-secure-smart-contract-code" id="how-to-write-more-secure-smart-contract-code"></a>

As a programmer, it is extremely important to understand of the security of your code. For developments on the ICON [Mainnet](broken-reference), your code will be available for public use. This means that businesses and users will depend on your code to execute in the intended way. Some common concerns are described here.

### Limitations of an audit

Many developers may be tempted to write some code, test it a little bit, then give it to an auditor with the hope that they will magically be able to identify and solve all of your security issues. While the process of auditing code is successful to discover [common software weaknesses and exploits](https://cwe.mitre.org), it does not protect against everything and is highly dependent on both the auditor to find everything and the developer to correctly fix all of the discovered problems.

### Smart contract development process <a href="#smart-contract-development-process" id="smart-contract-development-process"></a>

There are many different ways to successfully and securely develop code. Some of the common techniques are listed below so that you have the highest chances of success:

* Use a version control system, such as git, to store your code
* Use Pull Requests when you want to modify code
* Make sure that all of your code modifications are reviewed by at least one other person. If you are developing by yourself, look for someone else to trade reviews with
* Use a reliable development environment with simple commands
* Use a [code analysis tool](https://github.com/akullpp/awesome-java#code-analysis) before submitting a Pull Request
* Make sure that your compiler does not emit any warnings
* Document your code!!!

### Attacks and vulnerabilities <a href="#attacks-and-vulnerabilities" id="attacks-and-vulnerabilities"></a>

Here are some common attacks and vulnerabilities:

* Re-entrancy
* Front-running
* Integer overflow/underflow

### Further reading

[Smart Contract Weakness Classification and Test Cases](https://swcregistry.io)

[Java code analysis tools](https://github.com/akullpp/awesome-java#code-analysis)
