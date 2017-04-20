# Parser

For the full list of instructions for the paser please refer to the
[CS 331](http://www.cs.vassar.edu/~cs331) course page.

Once again the project uses TDD (Test Driven Development); the default 
project contains a suite of unit tests.  Your job is to get all the tests
to pass.  Feel free to write additional tests, in particular tests that
should be rejected by the parser.

## Project Setup

The parser assignment depends on your `LexicalAnalyzer` assignment and 
Maven needs to be able to find the jar file from assignment one. You
have two options:

1. Use your own Lexical analyzer (recommended for maximum points). See below 
for instuctions on configuring Maven to be able to find you lexical 
analyzer.
1. Use the model solution that is deployed to Maven. By default the project
is configured to use the model solution for assignment one so nothing
needs to be done if you want to use this option.

## Using Your Own Lexical Analyzer

To use your own lexical analyzer you must first:  

1. Install your lexical analzyer into your local repository (~/.m2/repository)
  by running the command `mvn install` in your lexical anaylzer project
  directory
1. Modify the pom.xml file to use your lexical analyzer instead of the
  model solution.  The `groupId` for the model solution is `lexical-analyzer-model`
  and you simply need to remove the `-model` bit.  The dependency for 
  the lexical analyzer should now look like:
  
```xml
<dependency>
    <groupId>edu.vassar.cs.cmpu331</groupId>    
    <artifactId>lexical-analyzer</artifactId>
    <version>1.0.0</version>
</dependency>
```

**NOTE** If (when) you find and fix bugs in your lexical analyzer you will
need to run `mvn install` again to install the fixed version into your
local repository.

**Tip** It is recommended to use the model solution for the lexical analyzer
until your parser is working.  This will allow you to focus on the parser and
isolate parser bugs from lexer bugs. Once you've got it working with the model solution,
you can begin to test with your own lexical analyzer.

## Bonus: Continuous Integrationg with Travis-CI

The project already comes with all the configuration files needed for 
[Travis-CI](https://education.travis-ci.com) to build your project whenever
code is pushed to GitHub.  The only wrinkle is that Travis will not have access
to your lexical analyzer; you will have to deploy your lexical analyzer to a public Maven repository that Travis
can access.

Fortunately, GitHub makes it easy to host an ad-hoc public repository that
Travis can locate.

### Creating an Ad-Hoc Maven Repository.

1. Login to GitHub and create a public repository under your own user account.
  Name the repository `mvn-repo`.
1. Clone this repository to your computer.
  
  ```
  $> git clone https://github.com/your-name/mvn-repo.git
  ```
1. Install your lexical analyzer jar into the mvn-repo directory you just cloned.
  
  ```
  $> cd [lexical-analyzer] 
  $> mvn clean package
  $> mvn install:install-file -DpomFile=pom.xml -Dfile=target/lexical-analyzer-1.0.0.jar -DlocalRepositoryPath=[repo]  
  ```

  Where:

  1. `[lexical-analyzer]` is the directory containing your lexical analyzer project.
  1. `[repo]` is the directory containing the `mvn-repo` you cloned from GitHub.
  
  **NOTE** For Windows users the `-Dfile` path will be `target\lexical-analyzer-1.0.0.jar`.
1. Push your local `mvn-repo` to GitHub.

  ```
  $> cd [mvn-repo]
  $> git push https://github.com/your-name/mvn-repo
  ```
1. Edit the `pom.xml` file for the parser project:
  1. Change the `username` property to your GitHub username. e.g.
  ```xml
  <properties>
      <username>your-github-username</username>
  </properties>
  ```
  1. Uncomment the `<repository>` at the end of the file.  That is delete
  the line that starts with &lt;!-- and the line that starts with --&gt;
  
**Note** If you make changes to your lexical analzyer don't forget to push the
updated version to your `mvn-repo` on GitHub or Travis will continue to
use the old version.

Once the setup is complete send an email to the Google Group to have Travis enabled for your repository. After pushing code to your master branch you should be able to view the build status on the [Travis server](https://travis-ci.com/vassar-cmpu-331).
