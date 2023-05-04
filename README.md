Download Link: https://assignmentchef.com/product/solved-csc133-assignment-3-interactive-graphics-and-animation
<br>
<h2>1. Introduction</h2>

The purpose of this assignment is to help you gain experience with interactive graphics and animation techniques such as repainting, timer-driven animation, collision detection, and object selection. Specifically, you are to make the following modifications to your game:

<ul>

 <li>the game world map is to display in the GUI (in addition to the text form on the console),</li>

 <li>the movement (animation) of game objects is to be driven by a timer,</li>

 <li>the game is to support dynamic collision detection and response,</li>

 <li>the game is to support simple interactive editing of some of the objects in the world, and</li>

 <li>the game is to include sounds appropriate to collisions and other events.</li>

</ul>

<h2>2. Game World Map</h2>

If you did Assignment#2 (A#2) properly, your program included an instance of a  <strong>MapView</strong> class which is an observer that displayed the game elements <em>on the console</em>. <strong>MapView</strong> also extended <strong>Container</strong> and that panel was placed in the middle of the game frame, although it was empty.

For this assignment, <strong>MapView</strong> will display the contents of the game <em>graphically</em> in the container in the middle of the game screen (in addition to displaying it in the text form on the console). When the <strong>MapView update()</strong> is invoked, it should now also should call <strong>repaint()</strong> on itself. As described in the course notes,  <strong>MapView</strong> should override <strong>paint()</strong>, which will be invoked as a result of calling <strong>repaint()</strong>. It is then the duty of  <strong>paint()</strong> to iterate through the game objects invoking <strong>draw()</strong> in each object – thus redrawing all the objects in the world in the container. Note that <strong>paint()</strong> must have access to the <strong>GameWorld</strong>. That means that the reference to the <strong>GameWorld</strong> must be saved when <strong>MapView</strong> is constructed, or alternatively the <strong>update()</strong> method must save it prior to calling <strong>repaint()</strong>.  Note that the modified <strong>MapView</strong> class communicates with the rest of the program <em>exactly</em> as it did previously (e.g., it is an observer of <strong>GameWorld</strong>).

As indicated in A#1, each type of game object has a different shape which can be bounded by a square. The size attribute provides the length of this bounding square. The different graphical representation of each game object is as follows: Bases are filled isosceles triangles; energy stations are filled circle; the player robot is a filled square while non-player robots are unfilled square; and drones are unfilled isosceles triangles. Hence, the size attribute of an energy station indicates the diameter of the circle, size of a base or drone indicates the length of the unequal side and height of the isosceles triangle, and size of a robot indicates the length of equal sides of the square.




Bases should include a text showing their number; energy stations should include a text showing their capacity.  Use the <strong>Graphics</strong> method <strong>drawString()</strong> to draw the text on bases and energy stations.

The appropriate place to put the responsibility for drawing each shape is within each type of game object (that is, to use a <em>polymorphic</em> drawing capability). The program should define a new interface named <strong>IDrawable </strong>specifying a method <strong>draw(Graphics g,  Point pCmpRelPrnt)</strong>. <strong>GameObject</strong> class should implement this interface and each concrete game object class should then provide code for drawing that particular object using

the received <strong>Graphics</strong> object <strong>g</strong> (which belong to <strong>MapView</strong>) and <strong>Point </strong>object<strong> pCmpRelPrnt</strong>, which is the component location (<strong>MapView</strong>’s origin location which is located at its the upper left corner) relative to its parent container’s origin (parent of <strong>MapView</strong> is the content pane of the <strong>Game</strong> form and origin of the parent is also located at its upper left corner). Remember that calling <strong>getX()</strong> and <strong>getY()</strong> methods on <strong>MapView</strong> would return the <strong>MapView</strong> component’s location relative to its parent container’s origin.

Each object’s <strong>draw() </strong>method draws the object in its current color and size, at its current location. Recall that current location of the object is defined relative to the origin of the game world (which corresponds to the origin of the <strong>MapView</strong> in A#2 and A#3). Hence, do not forget to add <strong>MapView</strong>’s origin location (relative to its parent container’s origin) to the current location while drawing your game objects since <strong>draw…()</strong> methods (e.g., <strong>drawRect()</strong>) of <strong>Graphics</strong> expects coordinates which are relative to the parent container’s origin. Also, recall that the <em>location</em> of each object is the position of the <em>center </em>of that object. Each <strong>draw() </strong>method must take this definition into account when drawing an object.  Remember that the <strong>draw…()</strong> method of the <strong>Graphics</strong> class expects to be given the X,Y coordinates of the <em>upper left corner</em> of the shape to be drawn. Thus, a <strong>draw()</strong> method would need to use the <em>location</em> and <em>size </em>attributes of the object to determine where to draw the object so its center coincides with its <em>location</em> (i.e., the X,Y coordinate of the upper left corner of a game object would be at<strong> center_location.x – size/2, center_location.y – size/2</strong> relative to the origin of the <strong>MapView,</strong> which is the origin of the game world).

<h2>3. Animation Control</h2>

The <strong>Game</strong> class is to include a timer (you should use the <strong>UITimer</strong>, a build-in CN1 class) to drive the animation (movement of movable objects). <strong>Game</strong> should also implement <strong>Runnable</strong> (a build-in CN1 interface). Each tick generated by the timer should call the <strong>run()</strong> method in <strong>Game</strong>.  <strong>run()</strong> in turn can then invoke the “Tick” method in <strong>GameWorld</strong> from the previous assignment, causing all moveable objects to move. This replaces the “Tick” button, which is no longer needed and should be eliminated.

There are some changes in the way the Tick method works for this assignment. In order for the animation to look smooth, the timer itself will have to tick at a fairly fast rate (about every 20 <em>msec</em> or so). In order for each movable object to know how far it should move, each timer tick should pass an “elapsed time” value to the <strong>move()</strong> method. The <strong>move()</strong> method should use this elapsed time value when it computes a new location.  For simplicity, you can simply pass the value of the timer event rate (e.g., 20 msec), rather than computing a true elapsed time. However, <em>it is a requirement that each  </em><strong>move() </strong><em>computes movement based on the value of the elapsed time parameter passed in</em>, not by assuming a hard-coded time value within the <strong>move()</strong> method itself. You should experiment to determine appropriate movement values (e.g., in A#1, we have specified the initial speed of the drone to be a random value between 5 and 10, you may need to adjust this range to make your drones have reasonable speed which is not to too fast or too slow). In addition, be aware that methods of the built-in CN1 <strong>Math</strong> class that you will use in <strong>move()</strong> method (e.g., <strong>Math.cos(), Math.sin()</strong>) expects the angles to be provided in radians not degrees. You can use <strong>Math.toRadians()</strong>to convert degrees to radians.   Likewise, the built-in <strong>MathUtil.atan()</strong> method that you might have used in the strategy classes also produces an angle in radians. You can use <strong>Math.toDegrees()</strong>to convert degrees to radians.

Remember that a <strong>UITimer</strong> starts as soon as its <strong>schedule()</strong> method is called. To stop a <strong>UITimer</strong> call its <strong>cancel()</strong> method. To re-start it call the <strong>schedule()</strong> method again.

<h2>4. Collision Detection and Response</h2>

There is another important thing that needs to happen on Tick method in <strong>GameWorld</strong>. After invoking  <strong>move()</strong> for all movable objects, your Tick method must determine if there are any collisions between objects, and if so to perform the appropriate “collision response”. You

must handle collision detection/response by having <strong>GameObject</strong> class implement a new

interface called “<strong>ICollider</strong>” which declares two methods

<strong>boolean collidesWith(GameObject otherObject) </strong>and

<strong>void handleCollision(GameObject otherObject) </strong>which are intended for performing collision detection and response, respectively.

In the previous assignment, collisions were caused by pressing one of the “pretend collision buttons” (i.e., “Collide With NPR”, “Collide With Base”,  “Collide With Drone”, “Collide with Energy Station”) and the objects involved in the collision were chosen arbitrarily. Now, the type of collision will be detected automatically during collision detection, so the pretend collision buttons are no longer needed and should be removed. Collision detection will require objects to check to see if they have collided with other objects, so the actual collisions will no longer be arbitrary, but will correspond to actual collisions in the game world. There are more hints regarding collision detection in the notes below.

Collision response (that is, the specific action taken by an object when it collides with another object) will be similar as before. Hence, <strong>handleCollision() </strong>method of a game object should call the appropriate collision handling method in <strong>GameWorld</strong> from the previous assignment. Collisions also generate a <em>sound</em> (see below) and thus, collision handling methods in <strong>GameWorld</strong> should be updated accordingly.

In addition to the player robot, NPRs can also collide with other NPRs, bases, drones, and energy stations. The effects of these collisions on an NPR would be the same as the effects caused by these collisions on the player robot (i.e., collision with other NPR would increase damage level and fade color of both NPRs, collision with base would increase the <em>lastBaseReached</em> value of the NPR given that the base number is one more than the current <em>lastBaseReached</em> value, collision with a drone would increase the damage level of the NPR, collision with an energy station would increase NPR’s energy level, reduce capacity of the energy station to zero, fade the color of the energy station, and add a new energy station).

Finally, since we now automatically detect collisions, we do not need to (and thus, we should not) assign the next <em>lastBaseReached</em> value to NPRs each time “Change Strategies” button is hit. This value of the NPR will now be updated as a result of collisions with the bases. In addition, although we still assume that NPRs never run out of energy, collision between a NPR and an energy station would increase the energy level of the NPR.

<h2>5. Sound</h2>

You may add as many sounds into your game as you wish. However, you must implement particular, <u>clearly different</u> sounds for <em>at least</em> the following situations:

<ul>

 <li>when robots collides, e.g., a player robot-NPR or NPR-NPR collision happens (such as a crash sound),</li>

 <li>when a robot (NPR/player robot) collides with an energy station (such as an electric charge sound),</li>

 <li>any collision which results in the player robot losing a life (such as an explosion or dying sound),</li>

 <li>some sort of appropriate background sound that loops continuously during animation.</li>

</ul>




Sounds should only be played if the “Sound” attribute is “On”. Note that except for the “background” sound, sounds are played as a result of executing a collision.  Hence, you must play these sounds in collision methods in <strong>GameWorld</strong>.

You may use any sounds you like, as long as I can show the game to the Dean and your mother (in other words, they are not disgusting or obscene). Short, unique sounds tend to improve game playability by avoiding too much confusing sound overlap. Do not use copyrighted sounds. You may search the web to find these non-copyrighted sounds (e.g., <strong>www.findsounds.com</strong>).

You must copy the sound files <u>directly under the <em>src</em> directory</u> of your project for CN1 to locate them. You should add <strong>Sound</strong> and <strong>BGSound </strong>classes to your project to add a capability for playing regular and looping (e.g., background) sounds, respectively, as discussed in the lecture notes. These classes encapsulate given sound files by making use of <strong>InputStream</strong>, <strong>MediaManager, and Media</strong> build-in CN1 classes. In addition to these built-in classes, <strong>BGSound</strong> also utilizes <strong>Runnable</strong> build-in CN1 interface. You should create a single sound object in <strong>GameWorld</strong> for each audio file and when you need to play the same sound file, you should use this single instance (e.g., all robot and energy station collisions should use the same sound object).

<h2>6. Object Selection and Game Modes</h2>

In order for us to explore the Command design pattern more thoroughly, and to gain experience with graphical object selection, we are going to add an additional capability to the game. The game is to have two modes:  “<em>play</em>” and “<em>pause</em>”.  The normal game play with animation as implemented above is “play” mode. In “pause” mode, animation stops – the game objects don’t move, the clock stops, and the background looped sound also stops. Also, when in pause mode, the user can use the pointer to select some of the game objects as explained below.

Ability to select the game mode should be implemented via a new GUI command button that switches between “play” and “pause” modes (you should create an additional command class to handle action events generated by this button and set its target as <strong>Game</strong>). When the game first starts it should be in play mode, with the mode control button displaying the label “Pause” (indicating that pushing the button switches to pause mode). Pressing the Pause button switches the game to pause mode and changes the label on the button to “Play”, indicating that pressing the button again resumes play and puts the game back into play mode (also restarting the background sound, if sound is enabled).

<h2><em>– Object Selection </em></h2>

When in pause mode, the game must support the ability to interactively <em>select</em> objects.

To identify “<em>selectable”</em> objects, you should have those objects implement an interface called <strong>ISelectable</strong> which specifies the methods required to implement selection<strong>, </strong>as discussed in the lecture notes. Selecting an object allows the user to perform certain actions on the selected object. For this assignment, all <strong>Fixed</strong> objects (<em>Energy Stations and Bases) </em>are selectable. The selected energy station/base must be highlighted by drawing it as an un-filled circle/triangle (a normal energy station/base is drawn as a filled circle/triangle). Selection is only allowed in <em>pause </em>mode, and switching to <em>play </em>mode automatically “unselects” the object.

An individual object is selected by pressing the pointer on it.  Pressing on an object selects that object and “unselects” all other objects. Clicking in a location where there are no objects causes the selected object to become unselected. Remember that pointer (x,y) location received by overriding the <strong>pointerPressed()</strong> method of <strong>MapView</strong> is relative to screen origin. You can make this location relative to <strong>MapView</strong>’s parent’s origin by calling the following lines inside the <strong>pointerPressed()</strong> method:

<strong>x = x – getParent().getAbsoluteX() y = y – getParent().getAbsoluteY() </strong>




A new <strong><em>Position </em></strong>command (which should extend from <strong>Command</strong> as the other commands of the game) is to be added to the game, invocable from a new “Position” button. When the position command is invoked, the selected energy station or base is to be moved to a new location. This movement should be done by following the below steps in the given order: 1) Select the object 2) Hit Position button 3) Click on <strong>MapView</strong> to select a new position for the selected object 4) Selected object would appear in the new location. The position action should only be available while in pause mode, and should have no effect on unselected objects. Note that the new position command gives the user the ability to create an arbitrary track configuration; the game is no longer constrained to use the hard-coded initial layout provided when the game starts or life is lost.

To execute the position command properly in A#3, we remove the requirement that was introduced in A#1 that restricted all fixed game objects from changing location once they are created. Also, note that regardless of their capacity all energy stations are allowed to be moved.

<h2><em>– Command Enabling/Disabling </em></h2>







Commands should be enabled only when their functionality is appropriate. For example, the Position command should be disabled while in play mode; likewise, commands that involve playing the game (e.g., changing the player robot direction) should be disabled while in pause mode. Note that disabling a command should disable it for all invokers (buttons, keys, <em>and </em>menu items). Note also that a disabled button or menu item should still be <em>visible</em>; it just cannot be active (enabled). This is indicated by changing the appearance of the button or menu item. To disable a button or a menu item which is added to the form by

<strong>addComponentToSideMenu()</strong> use <strong>setEnabled()</strong> method of <strong>Button </strong>(remember that <strong>Checkbox</strong> is-a <strong>Button</strong>). To disable a menu item which is added to the form by <strong>addCommandToSideMenu()</strong> use <strong>setEnabled()</strong> method of<strong> Command</strong>. To disable a key, use <strong>removeKeyListener()</strong> method of <strong>Form</strong> (remember to re-add the key listener when the command is enabled). You can set disabled style of a button using <strong>getDisabledStyle().set…()</strong> methods on the <strong>Button</strong>.

<h2>Additional Notes</h2>




<ul>

 <li>Please make sure that in A#2 you have added a <strong>MapView</strong> object directly to the center of the form. Adding a <strong>Container</strong> object to the center and then adding a <strong>MapView</strong> object onto it would cause incorrect results when a default layout is used for this center container (e.g., you cannot see any of the game objects being drawn in the center of the form).</li>

 <li>To draw a un-filled and filled triangle you can use <strong>drawPolygon()/fillPolygon()</strong> methods of These methods expect the following parameters: <strong>xPoints</strong> which is an array of integers that has x coordinates of the corners, <strong>yPoints</strong> which is an array of integers that has y coordinates of the corners, <strong>nPoints</strong> which is the integer that indicates the number of corners of the polygon. For instance, to draw a triangle with corner coordinates (1,1) (3,1) (2,2), you should assign <strong>xPoints =</strong> {1,3,2}, <strong>yPoints =</strong> {1,1,2}, <strong>nPoints</strong> = 3.</li>

 <li>To draw a filled circle with radius <strong>r</strong> at location <strong>(x,y)</strong>use <strong>fillArc(x, y, 2*r, 2*r, 0, 360)</strong>.</li>

 <li>As before, the origin of the game world (which corresponds to the origin of the <strong>MapView</strong> for now) is considered to be in its <em>lower left</em> corner<em>. </em>Hence, the Y coordinate of the game world grows <em>upward</em> (Y values increase <em>upward</em>). However, origin of the <strong>MapView</strong> container is at its upper left corner and Y coordinate of the <strong>MapView</strong> grows <em>downward</em>. So when a game object moves north (e.g., it’s heading is 0 and hence, its Y values is increasing) in the game world, they would move up in the game world. However, due to the coordinate systems mismatch mentioned above, heading up in the game world will result in moving down on the <strong>MapView</strong> (screen). Hence your game will be displayed as upside down on the screen.  This also means that when we robots turn left they will go west in game world, but they will go east on the <strong>MapView</strong> (and turning right means going west on the <strong>MapView</strong>). In addition, triangles (e.g. bases and drones) will be drawn upside down in <strong>MapView</strong>. <u>Leave it</u> <u>like this</u> – we will fix it in A#4.</li>

 <li>The simple shape representations for game objects will produce some slightly weird visual effects in <strong>MapView</strong>. For example, squares or triangles will always be aligned with the X/Y axes even if the object being represented is moving at an angle relative to the axes.  This is acceptable for now; we will see how to fix it in A#4.</li>

 <li>You may adjust the size of game objects for playability if you wish; just don’t make them so large (or small) that the game becomes unwieldy.</li>

 <li>As indicated in A#2, boundaries of the game world are equal to dimensions of <strong>MapView</strong>. Hence, drones should consider the width and the height of <strong>MapView</strong> when they move, not to be out of boundaries.</li>

 <li>Because the sound can be turned on and off by the user (using the menu), and also turns on/off automatically with the pause/play button, you will need to test the various combinations. For example, turning off sound, then pressing pause, then pressing play, should result in the sound <strong><em>not</em> </strong>coming back on. There are several sequences to test.</li>

 <li>When two objects collide handling the collision should be done only once. For instance, when two robots collide (i.e., robot1 and robot2) the robots damage level should be increased only once, not twice. In the two nested loops of collision detection, <strong>collidesWith(robot2)</strong> and <strong>robot2.collidesWith(robot1)</strong> will both return true. However, if we handle the collision with <strong>robot1.handleCollision(robot2)</strong> we should not handle it again by calling <strong>robot2.handleCollision(robot1)</strong>. This problem is complicated by the fact that in most cases the same collision will be detected repeatedly as one object passes through the other object. Another complication is that more than two objects can collide simultaneously. One straight-forward way of solving this complicated problem, is to have each collidable object (that may involve in such a problem) keep a list of the objects that it is already colliding with. An object can then skip the collision handling for objects it is already colliding with. Of course, you’ll also have to remove objects from the list when they are no longer colliding.</li>

</ul>

Implementation of this solution would require you to have a <strong>Vector</strong> (or <strong>Arraylist</strong>) for each collidable object (i.e., all game objects in Robo-Track game) which we will call as “collision vector”. When collidable object obj1 collides with obj2, right after handling the collision, you need to add obj2 to collision vector of obj1. If obj2 is also a collidable object, you also need to add obj1 to collision vector of obj2. Each time you check for possible collisions (in each clock tick, after the moving the objects) you need to update the collision vectors. If the obj1 and obj2 are no longer colliding, you need to remove them from each other’s collision vectors. You can use <strong>remove()</strong> method of <strong>Vector</strong> to remove an object from a collision vector. If two objects are still colliding (passes through each other), you should not add them again to the collision vectors. You can use <strong>contains()</strong> method of <strong>Vector</strong> to check if the object is already in the collision vector or not. The <strong>contains()</strong> method is also useful for deciding whether to handle collision or not. For instance, if the collision vector of obj2 already contains obj1 (or collision vector of obj1 already contains obj2), it means that collision between obj1 and obj2 is already handled and should not be handled again.

<ul>

 <li>You should tweak the parameters of your program for playability after you get the animation working. Things that impact playability include object sizes and speeds, etc. Your game is expected to operate in a reasonably-playable manner.</li>

 <li>As before, you may not use a “GUI Builder” for this assignment.</li>

 <li>All functionality from the previous assignment must be retained unless it is explicitly changed or deleted by the requirements for this assignment.</li>

 <li>Your program must be contained in a CN1 project called A3Prj. You must create your project following the instructions given at “2 – Introduction to Mobile App Development and CN1” lecture notes posted at Canvas (Steps for Eclipse: 1) File → New → Project →</li>

</ul>

Codename One Project. 2) Give a project name “A3Prj” and uncheck “Java 8 project” 3) Hit “Next”. 4) Give a main class name “Starter”, package name “com.mycompany.a3”, and select a “native” theme, and “Hello World(Bare Bones)” template (for manual GUI building). 5) Hit “Finish”.). Further, <strong><em><u>you must verify that your program works properly from the</u> <u>command prompt </u></em></strong>before submitting it to Canvas (Get into the A3Prj directory and type

“java -cp distA3Prj.jar;JavaSE.jar com.codename1.impl.javase.Simulator com.mycompany.a3.Starter” for Windows machines. For the command line arguments of Mac OS/Linux machines please refer to the class notes.). Substantial penalties will be applied to submissions which do not work properly from the command prompt.

<h2>Deliverables</h2>

Submitting your program requires the same three steps as for A#1 and A#2 except that you <strong>do not </strong>need to submit a UML for A#3:

<ol>

 <li>Be sure to verify that your program works from the command prompt as explained above.</li>

 <li>Create a <em>single </em>file in “<u>ZIP</u>” format containing (1) the entire “<u>src” directory</u> under your CN1 project directory (called A3Prj) which includes source code (“.java”) for all the classes in your program and the audio files, and (2) the <u>jar</u> (located under the “A3Prj/dist” directory) which is automatically generated by CN1 and includes the compiled (“.class”) files for your program in a zipped format. Be sure to name your ZIP file as YourLastName-YourFirstNamea3.zip.</li>

 <li>Create a “<u>TEXT”</u> (i.e., not a pdf, doc etc.) file called “readme-a3.txt” that includes your name and whether you have tested your submission on a lab machine. You are <strong>strongly encouraged</strong> to run and test your program on a lab machine (Hydra terminal server is recommended) before submitting your project. If you have done this test, also specify the lab number and the name of the specific machine you have used in that lab (or if you have used Hyrda as recommended, just say it was tested on Hydra) in the text file. In addition, if you have done this test, be sure that you include the <em>src</em> folder and <em>jar</em> file generated/tested on the lab machine in the above-mentioned zip file. This information helps the grader in case your submission does not work on the machine (s)he uses. You may also include additional information you want to share with the grader in this text file. You will receive the grader comments on your text file when grades are posted.</li>

 <li>Login to <strong><em>Canvas</em></strong>, select “Assignment#3”, and upload your ZIP file and TEXT file separately (do <strong>NOT </strong>place this TEXT file inside the ZIP file). Also, be sure to take note of the requirement stated in the course syllabus for keeping a <em><u>backup copy</u></em> of all submitted work (save a copy of your ZIP and TEXT files).</li>

</ol>




<em><u>All submitted work must be strictly your own</u>! </em>

<h2>Appendix 1:  Sample GUI</h2>

The following shows an example of how a completed A#3 game might look. It has a control panel on the left, right, and bottom with the required command buttons. Notice that the bottom control container does not have the Collide With NPR, Collide With Base, Collide With Drone, Collide With Energy Station buttons as in A#2 GUI, instead it has the Pause/Play and Position buttons. Position button is disabled since the game here is in “Play” mode as indicated by the fact that the Pause/Play button shows “Pause”. <strong>MapView</strong> in the middle contains <em>4 bases </em>(blue filled triangles), <em>2 energy stations</em> (green filled circles), 1 player robot (red filled square), 3 NPRs (red unfilled squares), and 2 drones (black unfilled triangles). Bases and energy stations include a text showing their number and capacity, respectively. As indicated in A#1, all bases and all robots have the same size whereas sizes of the rest of the game objects are chosen randomly when created. The initial capacity of an energy station is proportional to its size. Note also that the world is “upside down” (e.g., the triangles are facing down). We will fix this in A#4.


