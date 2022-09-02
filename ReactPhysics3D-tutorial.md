
# Tutorial: how to use ReactPhysics3D with GLUT
[ReactPhysics3D](https://www.reactphysics3d.com/) is an open source C++ physics engine library that can be used in 3D simulations and games.
It's quite easy to use and you'll probably be familiar with some of its basic concepts if you used a game engine like Unity before.
In this tutorial we'll see how to build and install it, how to link it inside a Visual Studio project and how to use it together with GLUT.

Useful links:
- [GitHub repository](https://github.com/DanielChappuis/reactphysics3d)
- [Documentation](https://www.reactphysics3d.com/documentation.html)

## Building and installing the library

### Download pre-built library
If you don't need the latest version of the library you can skip the building step and download files from [here]() (these files were built using Debug mode and Win32 platform).
Once you've done that you can go to the [install section](#use-the-library-inside-your-vs-project).

### Build the library manually
Use the following command to clone the ReactPhysics3D repository.
```
git clone https://github.com/DanielChappuis/reactphysics3d.git
```

If you don't have it already you also need to downlaod and install [cmake](https://cmake.org/download/).

Once you have cloned the repo you have to open cmake-gui and put the path to source code (the root directory of the repo you just cloned) and the path to the destination folder of your choice. Then you can click "Configure".

<p  align="center">
  <img src="img/rp3d/cmake1.png" width="400px">
</p>

Then you have to select the generator and the platform you want to use to build the library. It is important that you choose the platform used by your project.

<p  align="center">
  <img src="img/rp3d/cmake2.png" width="400px">
</p>

Now you can change the generator settings, CMAKE_INSTALL_PREFIX is the folder where you will find library files in the end of the building process. If you check RP3D_COMPILE_TESTBED cmake will also generate the folder of the testbed application where you will find a .sln file containing a lot of more advanced examples, more about that in the [testbed section](https://www.reactphysics3d.com/usermanual#x1-7500014) of the user manual.

After that click "Generate" and cmake will create the project that you can use to build the library.

<p  align="center">
  <img src="img/rp3d/cmake3.png" width="400px">
</p>

Now open the .sln file inside the destination folder you set up before configuring with cmake.

<p  align="center">
  <img src="img/rp3d/open_sln.png" width="400px">
</p>

After that you have to choose the correct mode (Release/Debug), this also needs to match what you will use to build the project. You can also check that the selected platform is the right one.

Then you have to right click on the reactphysics3d project and after that on "Build". Visual Studio will now build the library, wait for it to finish.

<p  align="center">
  <img src="img/rp3d/build_rp3d.png" width="1000px">
</p>

To complete the process right click on the project "INSTALL" and build it.

<p  align="center">
  <img src="img/rp3d/build_install.png" width="1000px">
</p>

Now you will find all the necessary files to use the library in your project inside the folder you specified before generating with cmake (the CMAKE_INSTALL_PREFIX setting).

### Use the library inside your VS project
Now that you have all the files you need you can open your Visual Studio project and edit the following properties (be sure to edit properties of the right configuration).

Add the path to ReactPhysics3D's include directory inside the "Additional include directories" setting under the "C/C++" section of the properties.

<p  align="center">
  <img src="img/rp3d/include_dir.png" width="600px">
</p>

Add the path to ReactPhysics3D's lib directory inside the "Additional lib directories" setting under the "Linker->General" section of the properties.

<p  align="center">
  <img src="img/rp3d/lib_dir.png" width="600px">
</p>

Add the name of ReactPhysics3D's .lib file inside the "Additional dependencies" setting under the "Linker->Input" section of the properties.

<p  align="center">
  <img src="img/rp3d/lib_dep.png" width="600px">
</p>

Now you can copy and run the [HelloWorld](#reactphysics3d-helloworld) script from ReactPhysics3D to check that everything is working correctly.

<p  align="center">
  <img src="img/rp3d/hello_world.png" width="1000px">
</p>

For further details about building and installing the library you can check out section [4](https://www.reactphysics3d.com/usermanual#x1-50004) and [5](https://www.reactphysics3d.com/usermanual#x1-160005) of the user manual.

## Usage
In the user manual you can find detailed descriptions of how to use the various features of the library, [section 6](https://www.reactphysics3d.com/usermanual#x1-170006) is about what you have seen in the HelloWorld. There's also an [API documentation](https://www.reactphysics3d.com/documentation/api/html/index.html).

### Integration with GLUT

Now that you have ReactPhysics3D at your disposal you can build a basic application that applies physics to objects rendered using GLUT. The result will look like this.

Full code of the example [here](#integration-of-reactphysics3d-with-glut).

<p  align="center">
  <img src="img/rp3d/rp3d_glut_example.gif" width="600px">
</p>

You can start with a basic GLUT application that draws, for example, a cube and a floor.

To handle physics you have to start with a PhysicsCommon and a PhysicsWorld and then create a RigidBody, like in the HelloWorld.

Global variables:
```c++
PhysicsCommon physicsCommon;
PhysicsWorld *world = physicsCommon.createPhysicsWorld();
const decimal timeStep = 1.0f / 60.0f;

Vector3 cubeSize(0.5, 0.5, 0.5);
Vector3 cubeInitPosition(0, 0.5, 0);
Quaternion cubeInitOrientation = Quaternion::fromEulerAngles(DegToRad(10), DegToRad(30), 0);
RigidBody *cubeBody;

Vector3 floorSize(5, 0.4, 2);
Vector3 floorInitPosition(0, -0.8, 0);
Quaternion floorInitOrientation = Quaternion::identity();
RigidBody *floorBody;
```

Inside the init function:
```c++
Transform cubeTransform(cubeInitPosition, cubeInitOrientation);
cubeBody = world->createRigidBody(cubeTransform);

Transform floorTransform(floorInitPosition, floorInitOrientation);
floorBody = world->createRigidBody(floorTransform);
```

For this example you also need to account for collisions, so after creating the RigidBody you have to add a collider to it. The last step for what concerns initialization is to set floor's RigidBody type to "static".

```c++
BoxShape *cubeShape = physicsCommon.createBoxShape(cubeSize * 0.5);
cubeBody->addCollider(cubeShape, rp3d::Transform::identity());

BoxShape *floorShape = physicsCommon.createBoxShape(floorSize * 0.5);
floorBody->addCollider(floorShape, rp3d::Transform::identity());
floorBody->setType(BodyType::STATIC);
```

Now that the RigidBodies' setup is complete you just need to call the update function of the PhysicsWorld inside the idle callback.

```c++
world->update(timeStep);
glutPostRedisplay();
```

The next step is to draw GLUT primitives in the right places, to do that you can (inside the display callback) get the current transform of the RigidBody, extract the corresponding homogeneus transformation matrix from it and multiply the matrix by the primitive you need to draw.

```c++
float cubeHTM[16];
Transform cubeTransform = cubeBody->getTransform();
cubeTransform.getOpenGLMatrix(cubeHTM);
glMultMatrixf(cubeHTM);
glScalef(cubeSize.x, cubeSize.y, cubeSize.z);
glutSolidCube(1);
```

To complete this basic example you can also set a couple of custom properties for the cube's RigidBody.

Inside the init function:
```c++
cubeBody->setMass(0.8);
cubeBody->getCollider(0)->getMaterial().setBounciness(0.4);
```

Congrats! You have completed the tutorial.

## Examples
### ReactPhysics3D HelloWorld
```c++
// Libraries 
#include <reactphysics3d/reactphysics3d.h> 
#include <iostream> 
 
// ReactPhysics3D namespace 
using namespace reactphysics3d; 
 
// Main function 
int main(int argc, char** argv) { 
 
    // First you need to create the PhysicsCommon object. 
    // This is a factory module that you can use to create physics 
    // world and other objects. It is also responsible for 
    // logging and memory management 
    PhysicsCommon physicsCommon; 
 
    // Create a physics world 
    PhysicsWorld* world = physicsCommon.createPhysicsWorld(); 
 
    // Create a rigid body in the world 
    Vector3 position(0, 20, 0); 
    Quaternion orientation = Quaternion::identity(); 
    Transform transform(position, orientation); 
    RigidBody* body = world->createRigidBody(transform); 
 
    const decimal timeStep = 1.0f / 60.0f; 
 
    // Step the simulation a few steps 
    for (int i=0; i < 20; i++) { 
 
        world->update(timeStep); 
 
        // Get the updated position of the body 
        const Transform& transform = body->getTransform(); 
        const Vector3& position = transform.getPosition(); 
 
        // Display the position of the body 
        std::cout << "Body Position: (" << position.x << ", " << 
      position.y << ", " << position.z << ")" << std::endl; 
    } 
 
    return 0; 
}
```

### Integration of ReactPhysics3D with GLUT
```c++
/* 
	Example of ReactPhysics3D integration with GLUT
 */

#include <reactphysics3d/reactphysics3d.h>
#include <windows.h>
#include <GL/glut.h>
#include <stdlib.h>
#include <iostream>

#define PI 3.141592654
#define DegToRad(deg) (deg * PI / 180)

#define XRES 800
#define YRES 800
float Vx_min = -1.0, Vx_max = 1.0;
float Vy_min = -1.0, Vy_max = 1.0;

bool simulate = true;

// Materials
GLfloat mat_solid_cyan[] = {0.2, 0.6, 0.6, 1.0};
GLfloat mat_solid_dark[] = {0.1, 0.1, 0.1, 1.0};
GLfloat mat_emission_zero[] = {0.0, 0.0, 0.0, 1.0};

// Lights
GLfloat light_ambient[] = {0.0, 0.0, 0.0, 1.0};
GLfloat light_diffuse[] = {1.0, 1.0, 1.0, 1.0};
GLfloat light_position[] = {0.0, 0.0, 12.0, 0.0};
GLfloat light_lmodel_amb[] = {0.4, 0.4, 0.4, 1.0};
GLfloat light_local_view[] = {0.0};

// Physics
using namespace rp3d;

PhysicsCommon physicsCommon;
PhysicsWorld *world = physicsCommon.createPhysicsWorld();
const decimal timeStep = 1.0f / 60.0f;

// Cube
Vector3 cubeSize(0.5, 0.5, 0.5);
Vector3 cubeInitPosition(0, 0.5, 0);
Quaternion cubeInitOrientation = Quaternion::fromEulerAngles(DegToRad(10), DegToRad(30), 0);
RigidBody *cubeBody;

// Floor
Vector3 floorSize(5, 0.4, 2);
Vector3 floorInitPosition(0, -0.8, 0);
Quaternion floorInitOrientation = Quaternion::identity();
RigidBody *floorBody;

void init(void)
{
	glClearColor(0.95, 0.95, 0.95, 0.0);
	glClear(GL_COLOR_BUFFER_BIT);	

	// Init lighting
	glEnable(GL_DEPTH_TEST);
	glLightfv(GL_LIGHT0, GL_AMBIENT, light_ambient);
	glLightfv(GL_LIGHT0, GL_DIFFUSE, light_diffuse);
	glLightfv(GL_LIGHT0, GL_POSITION, light_position);
	glLightModelfv(GL_LIGHT_MODEL_AMBIENT, light_lmodel_amb);
	glLightModelfv(GL_LIGHT_MODEL_LOCAL_VIEWER, light_local_view);
	glEnable(GL_LIGHTING);
	glEnable(GL_LIGHT0);

	// Init physics objects
	// Cube
	Transform cubeTransform(cubeInitPosition, cubeInitOrientation);				// Define initial transform of the object
	cubeBody = world->createRigidBody(cubeTransform);							// Create rigidbody

	BoxShape *cubeShape = physicsCommon.createBoxShape(cubeSize * 0.5);			// Create collider shape
	cubeBody->addCollider(cubeShape, rp3d::Transform::identity());				// Add collider to rigidbody
	cubeBody->setMass(0.8);
	cubeBody->getCollider(0)->getMaterial().setBounciness(0.4);

	// Floor
	Transform floorTransform(floorInitPosition, floorInitOrientation);			// Define initial transform of the object
	floorBody = world->createRigidBody(floorTransform);							// Create rigidbody

	BoxShape *floorShape = physicsCommon.createBoxShape(floorSize * 0.5);		// Create collider shape
	floorBody->addCollider(floorShape, rp3d::Transform::identity());			// Add collider to rigidbody

	floorBody->setType(BodyType::STATIC);										// Set floor rigidbody type to static
}

void drawBoxFromRigidBody(Vector3 boxSize, RigidBody *boxBody, GLfloat matColor[4])
{
	glMaterialfv(GL_FRONT, GL_DIFFUSE, matColor);

	glPushMatrix();
	glLoadIdentity();

	float boxHTM[16];
	Transform boxTransform = boxBody->getTransform();		// Get current rigidbody transform
	boxTransform.getOpenGLMatrix(boxHTM);					// Get homogeneus transformation matrix from transform
	glMultMatrixf(boxHTM);									// Multiply the matrix to move and rotate the object

	glScalef(boxSize.x, boxSize.y, boxSize.z);				// Draw a generic box scaling a cube
	glutSolidCube(1);

	glPopMatrix();
}

void display(void)
{
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);	
	glMaterialfv(GL_FRONT, GL_EMISSION, mat_emission_zero);

	drawBoxFromRigidBody(cubeSize, cubeBody, mat_solid_cyan);		// Draw cube
	drawBoxFromRigidBody(floorSize, floorBody, mat_solid_dark);		// Draw floor

	glutSwapBuffers();
}

void idle(void)
{
	if (simulate)
	{
		world->update(timeStep);		// Update physics world
		glutPostRedisplay();
	}
}

void myReshape(GLsizei w, GLsizei h)
{
	glViewport(0, 0, (GLsizei)w, (GLsizei)h);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	glOrtho(Vx_min, Vx_max, Vy_min, Vy_max, -1.0, 1.0);
	glMatrixMode(GL_MODELVIEW);
}

void keyboard(unsigned char key, int x, int y)
{
	switch (key) {
	case 27: // ESC
		exit(1);
		break;
	case 32: // SPACEBAR
		simulate = !simulate;
	default:
		break;
	}
}

int main(int argc, char **argv)
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB);
	glutInitWindowSize(XRES, YRES);
	glutCreateWindow("Example rp3d + glut");

	init();

	glutDisplayFunc(display);
	glutReshapeFunc(myReshape);
	glutIdleFunc(idle);
	glutKeyboardFunc(keyboard);
	glutMainLoop();
	return 0;
}
```
