# pybind11-template

This project takes the project in [this repo](https://github.com/smrfeld/cmake_cpp_pybind11_tutorial) and changes it in order to install it as a Python package using the recipe available [here](https://github.com/scikit-build/scikit-build-sample-projects/tree/master/projects/hello-pybind11). 
The main goal would be to have a quick guideline to make [Combine](https://github.com/cms-analysis/HiggsAnalysis-CombinedLimit) acceptably integrated with the Python ecosystem.

By simply running 
```bash
$ python setup.py install
``` 
a package ```automobile``` is installed and the following operations can be tried:
```python
import automobile
c = automobile.Motorcycle("Yamaha")
print("Made a motorcycle called: %s" % c.get_name())
c.ride("mullholland")
```

## Structure
The package is structured as follows:
```
.  
├── automobile  
│ └── __init__.py  
├── CMakeLists.txt  
├── cpp  
│ ├── CMakeLists.txt  
│ ├── include  
│ │ ├── automobile  
│ │ └── automobile_bits  
│ │ └── motorcycle.hpp  
│ ├── src  
│ │ └── motorcycle.cpp  
│ └── tests  
│ ├── bin  
│ │ └── test  
│ ├── CMakeLists.txt  
│ └── src  
│ └── test.cpp  
├── python  
│ ├── automobile_py.cpp  
│ └── motorcycle_py.cpp  
├── README.md  
├── setup.py  
└── tests  
└── test.py
```

where:

- ```automobile``` at the top level is the pure Python package that will be installed in the Python library directory (i.e. site-packages etc.)
- ```cpp``` contains the C++ code from wich we want to build the Python extensions
		- ```automobile``` here is just a header interface to avoid to include all the modules individually 
		- the ```CMakeLists.txt``` files in this part can be avoided and are useless for the purpose, they're there only to build and install the C++ library standalone
		- ```tests``` can be ignored as well
- ```python``` contains the "glue" between the C++ code and the Python world

## Technical Tips

Useful to remember:

- the C++ extensions are installed in a Python package called ```_automobile```; from this package, as can be seen in ```automobile/__init__.py```, we import all the classes in order to allow something like ```from automobile import Motorcycle```
- what mentioned above is taken care of in ```CMakeLists.txt``` and ```setup.py```, with the following lines:
		- ```install(TARGETS ${python_module_name} DESTINATION automobile)```
		- ```cmake_install_dir='.'```
		this path is relative to ```_skbuild/linux-x86_64-3.7/cmake-install``` created inside the build directory
