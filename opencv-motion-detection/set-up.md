# Set Up

OpenCV Motion Detection Project Set Up

Clone / create project repository:

```text
$ cd myproject
```

Create the Python Environment \(Python 3.7 as this is the latest OpenCv supports:

```text
$ pipenv --python 3.7
```

Or, add the required packages to the project:

```text
$ pipenv install opencv-contrib-python
$ pipenv install imutils

```

This will create a `Pipfile` if one doesn’t exist. If one does exist, it will automatically be edited with the new package you provided.

Next, activate the Pipenv shell:

```text
$ pipenv shell
$ python --version
```

This will spawn a new shell subprocess, which can be deactivated by using `exit`.  


