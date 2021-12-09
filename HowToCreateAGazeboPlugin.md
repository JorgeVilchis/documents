# How to create a Gazebo plugin

1. It is recommended to have a structure like this:

```
gazebo_project
   |--> model
   |--> world
   |--> script
```

The ```model``` directory contains the model of the robot, generally in simulation description file (SDF).

```world``` directory contains the world definition in SDF format as well.

```script``` contains the plug plugins in c++.

2. The next is a very basic templare of a gazebo plugin in c++.

```
#include <gazebo/gazebo.hh>
#include <iostream>
using std::cout;
using std::endl;

namespace gazebo {
    class WorldPluginMyRobot : public WorldPlugin
    {
        public:
         WorldPluginMyRobot() : WorldPlugin()
         {
             cout <<"Hello World!"<<endl;
         }
         void Load(physics::WorldPtr _world, sdf::ElementPtr _sdf)
         {}
         GZ_REGISTER_WORLD_PLUGIN(WorldPluginMyRobot)
    };

}
```
3. In the ```gazebo_project``` directory create a ```CMakelists.txt``` file with the next content:

```
cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

find_package(gazebo REQUIRED)
include_directories(${GAZEBO_INCLUDE_DIRS})
link_directorries(${GAZEBO_LIBRARY_DIRS})
list(APPEND CMAKE_CXX_FLAGS "${GAZEBO_CXX_FLAGS}")

add_library(hello SHARED script/hello.cpp)
target_link_libraries(hello ${GAZEBO_LIBRARIES})
```

4. Compile following standard cmake compilation.

5. In CLI type:

```
export GAZEBO_PLUGIN_PATH=${GAZEBO_PLUGIN_PATH}:/home/workspace/myrobot/build
```

6. In the  ```world``` directory edit the world SDF file and add the plugin call just after ```<world name="...">``` line.

```
<plugin name="hello" filename="libhello.so"/>
```
