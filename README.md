# How to used Doxygen with GitHUB

This is a simple demo for hosting Doxygen Code on Github.  All of the instructions herein are based on the command line and was tested on a mac.

Much of the information here was adapted from [Rick Foos](http://rickfoosusa.blogspot.com/2011/10/howto-use-doxygen-with-github.html) blog on the subject.

This [GitHUB project](https://github.com/smartell/APIDemo) acts as a simple example.  Another simple example can be found at at [MSEDemo](https://github.com/smartell/MSEdemo), and the corresponding Doxygen generated documentation at [MSEDemoAPI](http://smartell.github.io/MSEdemo/)


## Doxygen
[Doxygen](http://www.stack.nl/~dimitri/doxygen/) is a tool for generating documentation from annotated C++ source files, and it also supports other platforms as well. You must have a copy of Doxygen installed on your computer, as this program is necessary to generate the html files from your annotated C++ code.

## Git

[Git](http://git-scm.com) is a free open source distributed version control system that is now widely used by many of major software developers for version control (e.g., Goolge, Netflix, Microsoft).  It was originally developed for maintaining the Linux kerenel.  [GitHUB](https://github.com) is a web-based repository for storing your projects (such as this one) and is free for open source projects. You will also need to install Git on your computer and setup a GitHUB account if you do not already have one.

* * *
## INSTRUCTIONS
The following are a series of ([Terminal](http://en.wikipedia.org/wiki/Terminal_(OS_X))  instructions for setting up and hosting Doxygen output in the GitHUB pages section of your project.  It is important to note here that _GitHub_ is a code repository, and _GitHub Pages_ is seperate website to host your Doxygen documentation.

* Create a new repository on GitHub.
	- Be sure to Initialize the repository with a README file so you can use `git clone` and clone the repository immediately to your local computer.

* On your new GitHub page, select the settings button on the far right, scroll down to GitHub Pages and select the __Automatic Page Genorator__ Button.
	- On the subsequent page scroll down and choose __Continue to Layouts__
	- Finally select the __PUBLISH__ button.  

	This step will create a GitHub pages section in your project that is normally populated with some html files that users can use to describe more about thier projects.  We will be replacing this section with the html files generated by Doxygen.

* Open up terminal and prepare to clone the project into a new directory.
	- The terminal code below should perform this task.
	- Be sure to replace the `:smartell/APIDemo.git` portion of the code below with your GitHub username and project space.

__Terminal__

	> mkdir APIdemo
	> git clone git@git@github.com:smartell/APIDemo.git APIdemo/
	> cd APIdemo
	> ls
	README.md

* Create a new `html` directory and add this directory to the .gitignore list.  This will set up a directory that contains the Doxygen html output and when you issue a `git commit` command, Git will ignore this directory.
	- Add the directory and .gitignore list to you repo.
	- Push the repo to you GitHub repository.

__Terminal__

	> mkdir html
	> echo "html/" > .gitignore
	> git add html .gitignore
	> git commit -m "Added html folder (which will contain the gh-pages branch) and .gitignore list"
	> git push origin master

* The next _critical_ step is to clone the repository again, but from inside the html directory.  This step will create a repository of a repository.
	- Note the second to last step creates a branch called `gh-pages` that will only exists in the html folder, and then switches to that branch.
	- Next, delete the master branch from within the html directory.  Note that you will recieve a warning about deleting the master branch.  Don't worry about it. 
	- Next, remove the existing html files that were generated by GitHub when you set up the GitHub pages in step 2 above.
	- Finally, add a simple ReadMe file for the gh-pages branch.

__Terminal__

	> cd html
	> git clone git@git@github.com:smartell/APIDemo.git .
	> git checkout origin/gh-pages -b gh-pages
	> git branch -d master
	> rm -r *
	> echo "# A simple README file for the gh-pages branch" > README.md
	> git add README.md
	> git commit -m"Replaced gh-pages html with simple readme"

At this point you've now set up your Git repository on your local machine that ignores the contents of the html folder.  Any file changes/additions that occur in that directory will be ignored, unless your current parent working directory is the html directory.

The next steps involve setting up Doxygen, running Doxygen then pushing the Doxygen html output upto GitHub Pages.

* Create a configuration file for Doxygen a Mainpage.dox file to display the content of your project on the Home page.
	- The third line of code will create a template configuration file for Doxygen called __Doxyfile__


__Terminal__

	> mkdir docs
	> cd docs/
	> Doxygen -s -g Doxyfile
	> cat <<EOF  > mainpage.dox
	> /**
	> @brief Documentation for my project
	> @author Steve Martell
	> @file Mainpage.dox
	> */
	> /**
	> @mainpage Demonstration API
	> This is a simple demonstration of getting Doxygen output to GitHub
	> */
	> EOF
	> 

* Next edit the Doxyfile to include the files/directories that Doxygen should parse for document gernation.  Scroll down to the _configuration options_ section and under the __INPUT__ heading add the mainpage.dox

	- Optionally you may go through and turn off the latex output, and play around with other default settings. For this example, only the INPUT option is modified.

__Doxyfile__

	# configuration options related to the input files
	#---------------------------------------------------------------------------
	INPUT                  = ./docs/mainpage.dox


* Next Run Doxygen and be sure the html output is saved in the html directory.
	- Next change to the html directory and add the generated html content to the git repository and commit.
	- Lastly push these changes to the gh-pages branch upto GitHub.

__Terminal__

	> cd ..
	> Doxygen ./docs/Doxyfile
	> cd html
	> git add .
	> git commit -m "Added Doxygen output to repo"
	> git push origin gh-pages


Thats it. You should now be able to visity the [GitHub Pages](http://smartell.github.io/APIDemo/) section of your repository and view the Doxygen html output.

Good Luck
___
