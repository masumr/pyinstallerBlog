<div align="center">

   ![python](./image/python.png)
</div>

Python, powerful and versatile as it is, lacks a few key capabilities out of the box. For one, Python provides no native mechanism for compiling a Python program into a standalone executable package.

To be fair, the original use case for Python never called for standalone packages. Python programs have, by and large, been run in-place on systems where a copy of the Python interpreter lived. But the surging popularity of Python has created greater demand for running Python apps on systems with no installed Python runtime.

Several third parties have engineered solutions for deploying standalone Python apps. The most popular solution of the bunch, and the most mature, is __PyInstaller__. __PyInstaller__ doesn’t make the process of packaging a Python app to go totally painless, but it goes a long way there.


## Waitress WSGI Server

Waitress is a pure-Python WSGI server. At a first glance it might not appear to be that much different than many others; however, its development philosophy separates it from the rest. Its aim for easing the production (and development) burden caused by web servers for Python web-application developers. 

The installation is pretty simple. It is highly recommended to create a virtual environment before you install Waitress via the pip install command:

```commandline
            pip install waitress
```

Then You need to first import waitress via the following command:
```commandline
            from waitress import serve
```

I will be using the app as the variable name for the Flask server. Modify this according to the name that you have set:

```
            app = Flask(__name__)
```

Comment out the app.run in your main server and add the following code. 

By default, Waitress binds to any IPv4 address on port 8080. You can omit the host and port arguments and just call serve with the WSGI app as a single argument:
```python
            serve(
            app.run()
               host="127.0.0.1",
               port=5000,
               threads=2
            )

```
The most commonly-used parameters for serve are as follows:

* __host__ — Hostname or IP address (string) on which to listen, default 127.0.0.1, which means “all IP addresses on this host”. May not be used with the listen parameter.
* __port__ — TCP port (integer) on which to listen, default 8080. May not be used with the listen parameter.
* __ipv4__ — Enable or disable IPv4 (boolean).
* __ipv6__ — Enable or disable IPv6 (boolean).
* __threads__ — The number of threads used to process application logic (integer). The default value is 4.

## Create Executable from Python Script using Pyinstaller
__PyInstaller__ can be used to create .exe files for Windows, .app files for Mac, and distributable packages for Linux. Optionally, it can create a single file which is more convenient for distributing, but takes slightly longer to start because it unzips itself.

The installation is pretty simple. It is highly recommended to create a virtual environment before you install via the pip install command.

```commandline
            pip install pyinstaller
```

This file is saved in build.sh and runs this file using the following command in the terminal.
```commandline
            ./build.sh
```
### For windows is:
```python
                pyinstaller wsgi.py \
                --onefile \
                --distpath "./bin" \
                --name "cheque-printer" \
                --add-data "cheque_db.sqlite3;." \
                --hidden-import waitress \
                --clean

```

### For mac is:
```python
                pyinstaller wsgi.py \
                --onefile \
                --distpath "./bin" \
                --add-binary="/System/Library/Frameworks/Tk.framework/Tk":"tk" \
                --add-binary="/System/Library/Frameworks/Tcl.framework/Tcl":"tcl" \
                --name "cheque-printer" \
                --add-data "cheque_db.sqlite3;." \
                --hidden-import waitress \
                --clean
```



### For ubuntu is:
```python
                pyinstaller wsgi.py \
                --onefile \
                --distpath "./bin" \
                --name "cheque-printer" \
                --add-data "cheque_db.sqlite3:." \
                --hidden-import waitress \
                --clean
```
The most commonly-used parameters for __build.sh__  are as follows:
*  __--name__ — Change the name of your executable.
* __--onefile__ — Package your entire application into a single executable file. The default options create a folder of dependencies and an executable, whereas --onefile keeps distribution easier by creating only an executable.
* __--hidden-import__ — List multiple top-level imports that PyInstaller was unable to detect automatically. This is one way to work around your code using import inside functions and __import__(). You can also use --hidden-import multiple times in the same command.
* __--add-data and --add-binary__ — Instruct PyInstaller to insert additional data or binary files into your build. This is useful when you want to bundle in configuration files, examples, or other non-code data. 

__PyInstaller__ is complicated under the hood and will create a lot of output. So, it’s important to know what to focus on first. Namely, the executable you can distribute to your users and potential debugging information. By default, the pyinstaller command will create a few things of interest:

* A __*.spec__ file
  
* A __build/ folder__
    *The __build/ folder__ is where __PyInstaller__ puts most of the metadata and internal bookkeeping for building your executable. 
    *The build folder can be useful for debugging, but unless you have problems, this folder can largely be ignored. 
* A dist/ folder
    *After building, you’ll end up with a dist/ folder similar to the following:
  ```
                    dist/
                    |
                    └── cheque-printer/
                        └── cheque-printer
    ```
  The __dist/ folder__ contains the final artifact you’ll want to ship to your users. Inside the __dist/ folder__, there is a folder named after your entry-point. So in this example, you’ll have a __dist/cheque-printer__ folder that contains all the dependencies and executable for our application.
  
  The executable to run is __dist/cheque-printer/cheque-printer__ or __dist/cheque-printer/cheque-printer.exe__ if you’re on Windows.


