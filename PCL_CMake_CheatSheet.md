# 🚀 PCL + CMake + Git Project Cheat Sheet

## 📋 Prerequisites Installation (One-time setup)

```bash
# Update system
sudo apt update && sudo apt upgrade

# Install essential build tools
sudo apt install -y build-essential cmake git

# Install PCL dependencies
sudo apt install -y libboost-all-dev libeigen3-dev libflann-dev libgtest-dev \
    libgoogle-glog-dev libgflags-dev libhdf5-dev libatlas-base-dev \
    libsuitesparse-dev libvtk9-dev libqhull-dev

# Install NVIDIA drivers (if you have NVIDIA GPU)
sudo apt install nvidia-driver-575
echo 'blacklist nouveau' | sudo tee -a /etc/modprobe.d/blacklist-nouveau.conf
sudo reboot  # Reboot to activate NVIDIA drivers
```

## 🔧 PCL Installation (Latest Version)

```bash
# Build PCL 1.13.1 from source (recommended)
cd /tmp
git clone https://github.com/PointCloudLibrary/pcl.git
cd pcl
git checkout pcl-1.13.1
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DPCL_QT_VERSION=5
make -j$(nproc)
sudo make install
```

## 📁 Project Structure Template

```
project-name/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   ├── processPointClouds.h
│   ├── processPointClouds.cpp
│   └── render/
│       ├── render.h
│       └── render.cpp
├── data/
│   └── pointclouds/
├── build/
└── README.md
```

## 🔨 CMakeLists.txt Template

```cmake
cmake_minimum_required(VERSION 3.5)

# Project Name and C++ Standard
project(your_project_name)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Compiler flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")

# Find PCL (Point Cloud Library)
find_package(PCL 1.13.1 REQUIRED)

# Include PCL headers and link libraries
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

# Optional: Remove vtkproj4 if it causes linking issues
list(REMOVE_ITEM PCL_LIBRARIES "vtkproj4")

# Create executable
add_executable(your_executable_name 
    src/main.cpp 
    src/render/render.cpp 
    # Add other source files here
)

# Link against PCL libraries
target_link_libraries(your_executable_name ${PCL_LIBRARIES})
```

## 🏗️ Build Commands

```bash
# Create build directory
mkdir build && cd build

# Configure with CMake
cmake ..

# Build the project
make

# Run the executable
./your_executable_name

# Clean build (if needed)
make clean

# Rebuild everything
make -B
```

## 🔍 Troubleshooting Common Issues

```bash
# Check PCL version
pkg-config --modversion pcl_common

# Check NVIDIA drivers
nvidia-smi

# Check PCL installation
find /usr -name "PCLConfig.cmake" 2>/dev/null

# Verify PCL libraries
ldconfig -p | grep pcl
```

## 📚 Git Workflow for Collaborative Development

### Initial Setup

```bash
# Configure Git (one-time setup)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Clone existing repository
git clone https://github.com/username/repository-name.git
cd repository-name

# Or initialize new repository
git init
git remote add origin https://github.com/username/repository-name.git
```

### Daily Workflow

```bash
# Check status
git status

# Pull latest changes from remote
git pull origin main

# Create a new branch for your feature
git checkout -b feature/your-feature-name

# Make your changes, then stage them
git add .

# Commit your changes
git commit -m "Descriptive commit message"

# Push your branch to remote
git push origin feature/your-feature-name
```

### Collaborative Workflow

```bash
# Switch to main branch
git checkout main

# Pull latest changes
git pull origin main

# Create feature branch
git checkout -b feature/new-feature

# Make changes and commit
git add .
git commit -m "Add new feature"

# Push feature branch
git push origin feature/new-feature

# Create Pull Request on GitHub (via web interface)
# Then merge via GitHub interface

# After merge, update local main
git checkout main
git pull origin main

# Delete feature branch (optional)
git branch -d feature/new-feature
git push origin --delete feature/new-feature
```

### Useful Git Commands

```bash
# View commit history
git log --oneline -10

# View differences
git diff HEAD
git diff HEAD~1

# Stash changes temporarily
git stash
git stash pop

# Reset to previous commit
git reset --hard HEAD~1

# View remote branches
git branch -r

# Fetch all remote changes
git fetch --all

# Merge specific branch
git merge origin/feature-name
```

## 🚀 Quick Start Commands

### For New Projects

```bash
# 1. Create project structure
mkdir my-pcl-project && cd my-pcl-project
mkdir src data build

# 2. Initialize Git
git init
git remote add origin https://github.com/username/my-pcl-project.git

# 3. Create CMakeLists.txt (use template above)

# 4. Build and test
mkdir build && cd build
cmake ..
make
./your_executable_name

# 5. Commit and push
git add .
git commit -m "Initial project setup"
git push origin main
```

### For Existing Projects

```bash
# 1. Clone repository
git clone https://github.com/username/project-name.git
cd project-name

# 2. Build project
mkdir build && cd build
cmake ..
make

# 3. Run project
./executable_name
```

## 📝 Best Practices

1. **Always work on feature branches** - never commit directly to main
2. **Write descriptive commit messages** - explain what and why, not how
3. **Keep commits atomic** - one logical change per commit
4. **Pull before pushing** - always sync with remote before pushing
5. **Use meaningful branch names** - feature/description, bugfix/issue-number
6. **Test before committing** - ensure your code builds and runs
7. **Update documentation** - keep README and comments up to date

## 🔧 Environment Variables (Optional)

```bash
# Add to ~/.bashrc for easier PCL development
export PCL_ROOT=/usr/local
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$PCL_ROOT/lib
export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:$PCL_ROOT/lib/pkgconfig
```

## 📊 Performance Tips

```bash
# Use multiple cores for building
make -j$(nproc)

# Enable optimizations in CMake
cmake .. -DCMAKE_BUILD_TYPE=Release

# Profile your application
valgrind --tool=callgrind ./your_executable_name
```

---

**Remember**: This cheat sheet is based on the successful setup of the [sfnd-lidar-project](https://github.com/htaamneh29/sfnd-lidar-project.git) with PCL 1.13.1 and NVIDIA drivers on Ubuntu 22.04.
