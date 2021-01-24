# Virtual Environment
Currently, most of the web apps are no longer being developed in the local environment.  One of the most straightforward reasons is dependency management.  If you are using your local environment as the development environment, then, you need to guarantee that you have all the mandatory dependencies locally with correct versions.  In fact, this is a really annoying preparation for the development and you will need to manually uninstall those dependencies after the development which is also a painful work.  Thus, many developers start to move to do the development into the virtual environment. (In fact, if you have worked with a Pycharm project, the Pycharm will automatically create a virtual environment for you and dependencies will be installed in the virtual environment.)

## Usage
A virtual environment for Python is relatively easier to set up compare to an environmental environment for other languages.

1. Make sure you have installed pipenv
   - You can install the pipenv using pip

   - The pipenv is used for starting a virtual environment

2. Make sure there is a Pipfile

   - The Pipfile lists out all the required dependencies and their versions for the project as well as the required Python interpreter for the project.  At the bottom of the Pipfile there is a portion named script which is the place for storing some short cuts for running scripts in the virtual environment.

- When you have both of them you can start your project by firstly installing all the dependency in the Pipfile using `pipenv install`

- After the installation is completed you can choose to run your app whether
   - Entering the virtual shell by `pipenv shell` and start your code by `python <filename>`

   - If you have already written the script in the Pipfile, you can run app by `pipenv run <script>`




