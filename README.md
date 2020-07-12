- Summarized  as a small tool to boost working efficiency.
- Support multi-channel and 16-bit images, especially RGB-D pairs.
- Efficient for Real-time prediction based on PyTorch/Python with streaming device input built on C++ API.

# cpp_python_socket
Simple TCP/IP socket comunication wrapper between c++ and Python for IPC.

## General Information
- Branch **master** covers simple string communication, relies on standard libraries.
- Branch **image_transferring** adds image transferring capabilities, relies on:
1. OpenCV for c++: `https://github.com/opencv/opencvBuild`
2. OpenCV for Python: `pip install opencv-python`

Only tested in Ubuntu 16.04, write an issue should you find any error.

## Quick Start
1. Either clone or add as a submodule this repo to your project folder:

`git clone https://github.com/OleguerCanal/cpp_python_socket.git`
or

`git submodule add https://github.com/OleguerCanal/cpp_python_socket.git`

2. [OPTIONAL] Change branch to enable image transferring:
`cd cpp_python_socket/`

`git checkout image_transferring`

3. If intending to use C++ code, add this 3 things to your CMakeLists.txt:
- `add_subdirectory(cpp_python_socket)`
- Append `cpp_python_socket/cpp/include` to `include_directories(...`
- Append `cpp_sockets` to `target_link_libraries(...` of your library/executable.

## Test the code
1. Change directory
`cd cpp_python_socket/`

2. Build it:
`./build.sh`

3. Run unit test:
- Terminal 1: `python python/server.py`
- Terminal 2: `cd cpp;` `./run_cpp_client_test.sh`

## Usage examples
Python Server:
```Python
from cpp_python_socket.python.server import Server

if __name__ == "__main__":
  server = Server("127.0.0.1", 5001)
  message = server.receive()
  print("[CLIENT]:" + message)
  server.send("Shut up")
```

C++ client:
```cpp
#include <iostream>
#include "client.hpp"

int main() {
    socket_communication::Client client("127.0.0.1", 5001);  // ip, port
    client.Send("Hello hello!");  // Send string
    std::string answer = client.Receive();  // Receive string
    std::cout << "[SERVER]: " << answer << std::endl;
}
```
