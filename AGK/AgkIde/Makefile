# discover if this is a 32 or 64 bit machine
ARCH := $(shell getconf LONG_BIT)

CC = g++ 
ODIR = linuxbuild/obj$(ARCH)
INC = `pkg-config --cflags gtk+-3.0` -Icommon/include -Icommon -Ibullet -Ibullet/BulletCollision/CollisionShapes
CFLAGS = -O2 -DNDEBUG 
LDFLAGS32 = -Lplatform/linux/Lib/Release32
LDFLAGS64 = -Lplatform/linux/Lib/Release64
LDFLAGS = $(LDFLAGS$(ARCH)) -static-libstdc++ -static-libgcc -L../Broadcaster/AGKBroadcaster/build -L../CompilerNew/build --std=c++11 -no-pie

# list of dependencies so that changes to these files cause a rebuild of source files
DEPS = template.h

# list of object files to compile, they will be placed in the $(ODIR) folder
# matching .cpp files will be searched for below
_OBJS = CoreLinux.o \
        2DScene.o \
        files.o \
        Gui.o \
        Ide.o \
        Project.o  
OBJS = $(patsubst %,$(ODIR)/%,$(_OBJS))

# entry point, create folders, compile source files, and link the executable
all: setup $(OBJS) Executable

# make sure the build folders exist
setup:
	mkdir -p linuxbuild/obj$(ARCH)

# compile a source file, first search path is the template folder
$(ODIR)/%.o: %.cpp $(DEPS)
	$(CC) -DIDE_LINUX -DAGK_LINUX -D__x86_$(ARCH)_ -Wno-unused-result -static -no-pie -c -std=c++11 $(INC) -o $@ $< $(CFLAGS)

# link the .o files
Executable: | $(OBJS)
	$(CC) $(OBJS) -o linuxbuild/AgkStudio$(ARCH) $(LDFLAGS) -L/usr/lib/x86_64-linux-gnu/ `pkg-config --libs gtk+-3.0` -Wl,-Bstatic -lAGKLinux -lAGKBroadcasterIDE$(ARCH) -lAGKCompilerIDE$(ARCH) -lglfw3 -Wl,-Bdynamic -lGL -lXt -lX11 -lXxf86vm -lXrandr -lpthread -lXi -lXinerama -lXcursor -lopenal -ludev -ldl

# delete compiled files
clean:
	rm -rf linuxbuild/obj$(ARCH)/*
	rm -f linuxbuild/LinuxApp$(ARCH)


