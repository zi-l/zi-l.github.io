## Troubleshooting on Linux
##### ModuleNotFoundError: No module named 'tkinter'*
tkinter依赖问题     

ubuntu  
```
sudo apt-get install python3-tk
```
centos  
```
sudo yum install python3-tkinter
```

##### src/pyodbc.h: fatal error: Python.h/sql.h: No such file or directory
pyodbc依赖问题  

ubuntu  
```
sudo apt-get install unixodbc unixodbc-dev
```
centos  
```
sudo yum install epel-release
sudo yum install python3-pip gcc-c++ python3-devel unixODBC-devel
```

##### 