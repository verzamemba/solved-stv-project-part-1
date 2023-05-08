Download Link: https://assignmentchef.com/product/solved-stv-project-part-1
<br>
<span style="font-size: 2.61792em; letter-spacing: -1px;">1        Required software</span>

You need an IDE for C# that includes a code coverage tool. There are two options:

<ol>

 <li>Jetbrains Rider<sup>1</sup>. This has my preference. If you use Mac or Linux, you should use Rider. You can get free education license for this<sup>2</sup>.</li>

 <li>Microsoft Visual Studio Enterprise Edition. You need the <em><sub>Enterprise </sub></em> It is a bit overkill, but smaller edition does not include any code coverage tool. Microsoft supposedly also offer an education license for</li>

</ol>

<a href="https://www.jetbrains.com/rider/"><sup>1</sup></a><a href="https://www.jetbrains.com/rider/">https://www.jetbrains.com/rider/</a>

<a href="https://www.jetbrains.com/community/education/#students"><sup>2</sup></a><a href="https://www.jetbrains.com/community/education/#students">https://www.jetbrains.com/community/education/#students</a>

<em>Course Software Testing &amp; Verification, </em>2020.

this<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>. You can login with your Solis account. Unfortunately Microsoft also asks for your phonenumber, so if you don’t like this you should go with Rider.

For Unit Testing we will be using NUnit Testing Framework<sup>4</sup>, but your IDE should get this automatically when you read the project’s <sub>.sln </sub>file into the IDE.

You will also need a Code Metrics tool. An important metric is the McCabe/Cyclometic metric (Wikipedia gives an adequate explanation on this). If you use Rider you need to install the CyclomaticComplexity plugin. If you use Visual Studio for Windows, code metrics are already included.

<h1>2        The Game Logic</h1>

The game logic is implemented by the classes in STVrogue.GameLogic. A large part of these classes are left unimplemented for you. And yes, you will also need to test them to make sure you deliver a correct game logic.

2.1        Class GameEntity

Monsters, items, rooms, and the player are the main entities of the game. They will have their own class, but they all inherit from a class called <sub>GameEntity</sub>. We will insist that game entities (so, instances of <sub>GameEntity</sub>) should have <em>unique IDs</em>. This is will be in particular important for PART-2 of the Project.

2.2       Class Dungeon

The game is played on a dungeon, which consists of rooms. There are three types of rooms: <em>start-room</em>, <em>exit-room</em>, and other rooms (we will call then ’ordinary’ room). A dungeon should have one unique start-room and one unique exitroom.

Rooms are connected with edges. If <em><sub>r </sub></em>is a room, all rooms that are directly connected to <em>r </em>are called the <em><sub>neighbors </sub></em>of <em>r</em>. The player and monsters can move from rooms to rooms by traversing edges. Technically, this means that the rooms in the dungeon form a graph whose edges are bi-directional. We require that <em>all rooms in the dungeon are reachable from the start-node</em>. Figure 1 shows some example of dungeons. The class Dungeon has two basic operations: 1. A constructor Dungeon(<em>shape</em>,<em>N</em>,<em>γ</em>) to create a dungeon consisting of <em><sub>N</sub></em><sub>&gt;</sub>2 rooms.

<ol>

 <li>Keep in mind that a dungeon should satisfy the previously mentioned constraints about its connectivity and the uniqueness of its start and exit-rooms.</li>

</ol>

Figure 1. <em>Some examples of dungeons: tree-shaped (left), linear dungeon (top right), and a dungeon which is neither treeshaped nor linear (bottom right).</em>

<ol>

 <li>The parameter <em><sub>shape </sub></em>determines the shape of the dungeon. There are three types of shapes: <em><sub>linear</sub></em>, <em>tree</em>, and <em><sub>random</sub></em>. A linear-shaped dungeon forms a list with the start-room at one of its ends, and the exit-room at the other end.</li>

</ol>

A tree-shaped dungeon forms a tree with the startroom as the root. The exit-room should be a leaf of this tree.

A <em><sub>random </sub></em>dungeon can has any shape as long as it is not linear nor a tree.

<ol>

 <li>Every room in the dungeon has a <em><sub>capacity</sub></em>. If <em>c </em>is the capacity of a room <em><sub>r</sub></em>, the number of monsters in <em><sub>r </sub></em>should not exceed <em><sub>r</sub></em>. This capacity is a random value determined when the dungeon is created. For an ordinary room, it should be in [1<sub>..<em>γ</em></sub>]. Note that different rooms may thus have different capacity, but none will exceed the maximum capacity <em><sub>γ</sub></em>. The start and exit-rooms always have capacity 0.</li>

 <li>A method SeedMonstersAndItems(<em>M</em>,<em>H</em>,<em>R</em>) to randomly populate the rooms in the dungeon with monsters and items. There are two types of items: healing potion and rage potion.</li>

</ol>

The paramemer <em><sub>M </sub></em>specifies the number of monsters to be dropped in the dungeon, <em><sub>H </sub></em>is the number of healing potions to be dropped, and <em><sub>R </sub></em>is the number of rage potions.

Populating the dungeon are subject to the requirements set below. Meeting these requirements are not always possible (e.g. it is not possible to populate a dungeon with <em><sub>N </sub></em>rooms whose capacities are at most <em>k </em>with (<em>N </em><sub>− </sub>2)<em>k </em>monsters or more).

The method SeedMonstersAndItems returns true if it manages fullfill the requirements, else it returns false (and left the dungeon unpopulated).

<ol>

 <li>Every monster in the dungeon should be alive and have HP and AR <sub>&gt;</sub></li>

 <li>Every room cannot be populated with more monsters than its capacity allows.</li>

 <li>Let <em><sub>E </sub></em>be the set of neighbor rooms of the exit-room. So: <em>E </em>= <em>exitroom</em>.Neighbors. Every room in <em>E </em>should be populated with more monsters than any non-E room. So, for any<em>r </em>∈ <em>E </em>and<em>r</em><sup>′ </sup>&lt; <em>E</em>, then |<em>r</em>.monsters| &gt; |<em>r</em><sup>′</sup>.monsters| should hold.</li>

 <li>Let <em><sub>N </sub></em>be the number of rooms in the dungeon. At least ⌈<em><sub>N</sub></em><sub>/</sub>2⌉ number of rooms should have no item at all.</li>

 <li>Let <em>I </em>= <em>startroom</em>.Neighbors ∪ {<em>startroom</em>}. There should be at least one healing potion and one rage potion somewhere in <em><sub>I</sub></em>.</li>

</ol>

2.3      Class Creature

A creature has <em><sub>hit point </sub></em>(HP), attack rating, and its location

(the room it is in) in a dungeon. Attack rating should be a positive integer. A creature is <em><sub>alive </sub></em>if and only if its HP is <sub>&gt;</sub>0. There are two subclasses of Creature: <em><sub>Monster </sub></em>and <em><sub>Player</sub></em>.

Creature has two operations: <sub>move</sub>(<em>r</em>) to move it to a

neigboring room, subject to the room capacity, and <sub>attack</sub>(<em>f </em>) to attack another creature <em><sub>f </sub></em>provided it is located in the same room. When a creature<em><sub>c </sub></em>attacks <em><sub>f </sub></em>, the action will damage <em><sub>f </sub></em>’s HP (that is, reducing it) by <sub>∆ </sub>where <sub>∆ </sub>is the attacker’s attack rating. If <em><sub>f </sub></em>’s HP drops to 0, <em><sub>f </sub></em>dies. A base implementation of these two operations are already provided, though you will have to override it for Player, e.g. because player moves are not constrained by rooms’ capacity.

The player has additionally ’Kill Point’ (KP) that is increased by one each time it kills a monster. The player also has a bag, that contains items it picked up.

2.4      Items

Items are dropped in the dungeon. When the player enters a room that contains items, it can pick them. The items will then be put in the player’s bag.

There are two types of items: <em>Healing Potion </em>and <em>Rage Potion</em>. A healing potion has some positive healing value.

When used, it will restore the player’s HP with this value, though the HP can never be healed beyond the player’s

HPMax.

A rage potion will turn the player into a raging barbarian. This temporarily double the player’s attack rating. The effect last for 5 turns (including the turn when it is used), though it also has some consequence that will be explained later.

Using a potion will consume it.

<table width="675">

 <tbody>

  <tr>

   <td width="665">STV Project 2019/20, PART 1                                                                                                   Course Software Testing &amp; Verification,</td>

  </tr>

 </tbody>

</table>

2.5      Class Game

The class provides the entry point to the game logic, and also holds most of the game logic<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>.

The constructor <sub>Game</sub>(<em>conf </em>) takes a configuration and will create a populated dungeon according to the configuration. The configuration<em>conf </em>is a record (<em>shape</em>,<em>N</em>,<em>γ</em>,<em>M</em>,<em>H</em>,<em>R</em>,<em>dif </em>) of 7 parameters:

<ol>

 <li><em><sub>shape </sub></em>the shape of the dungeon to generate.</li>

 <li><em><sub>N </sub></em>the number of rooms in the dungeon.</li>

 <li><em><sub>γ </sub></em>specifies the maximum rooms’ capacity.</li>

 <li><em><sub>M </sub></em>is the number of monsters to generate.</li>

 <li><em><sub>H </sub></em>is the number of healing potion to generate.</li>

 <li><em><sub>R </sub></em>is the number of rage potion to generate.</li>

 <li><em><sub>dif </sub></em>is the difficulty mode of the game. There are three modes: <em>Newbie</em>-mode (easy), <em>Normal</em>-mode, and <em>Elite</em></li>

</ol>

The constructor will generate a dungeon satisfying the parameters in <em><sub>conf </sub></em>. Some configurations might be hard, or, as remarked in Section 2.2, even impossible to satisfy. The constructor should throw an exception if it fails, e.g. after several attemps, to generate a dungeon that satisfies the configuration.

All game entities in the generated dungeon should have unique IDs. The player should be alive, and its HP is equal to HPMax, and <sub>&gt;</sub>0. The player always starts at the start-room of the dungeon.

STV Rogue is a turn-based game. It means that the game moves from turn to turn, starting from turn 0, then turn 1, turn 2, etc. At a turn, every creature in the dungeon, and is still alive, makes one single action. The order is random, as long as everyone gets exacly one action. Between turns, the game state does not change.

The player wins if it manages to reach the dungeon’s exit-node. It loses if it dies before reaching it. The main API of the class <sub>Game </sub>is the method <sub>update</sub>(<em>α</em>). This will iterate over all creatures in the dungeon as said above. A monster can choose its action randomly; this will be explained more below. The action of the player is as specified by <em><sub>α </sub></em>(you can imagine that this <em><sub>α </sub></em>represents a choice that the actual human player indicated through the game UI).

The player is <em><sub>in-combat </sub></em>if it is in the same room with a monster. Likewise, a monster is in-combat if it is in the same room with the player.

There are six possible actions that a creature can do, though a monster can only do four of them:

<ol>

 <li>DoNOTHING, it means as it says.</li>

 <li><sub>MOVE </sub><em>r</em>: the creature moves to another node <em>r</em>. This should be a neighboring node, and furthermore this should not breach <em><sub>r</sub></em>’s capacity.</li>

</ol>

MOVE is <sub>not </sub>possible when the creature is in combat. The logic for executing this action is to be implemented in the method Game.Move(<em>c</em>,<em>r</em>), where <em>c </em>is the creature that moves.

<ol start="3">

 <li><sub>PICKUP</sub>: this will cause the player to pick up all items in the room it is currently at. The items will the be put in the player’s bag. A monster cannot do this action.</li>

 <li><sub>USE </sub><em><sub>i</sub></em>: this will cause the player to use an item <em><sub>i</sub></em>. The item should be in its bag. The effect of using different items were explained in Section 2.4. The logic for executing this action is to be implemented in the method Game.UseItem(<em>i</em>).</li>

 <li><sub>ATTACK </sub><em>f </em>: the creature attacks another creature <em>f </em>. This is only possible if both the attacker and defender are alive and are in the same room. Also, a monster cannot attack another monster. The logic for executing this action to be implemented in the method Game.Attack(<em>c</em>, <em>f </em>), where <em>c </em>is the attacker and <em><sub>f </sub></em>the defender.</li>

 <li><sub>FLEE</sub>: the creature flees a combat to a randomly chosen <em>neighboring </em> The conditions for when fleeing is allowed are a bit complicated. They are given below. When multiple conditions conflict, the condition that is listed first takes precedence (e.g. conditions c and d below may conflict; in such a situation we should follow c and ignore d).</li>

 <li>A monster cannot flee to a room if this would exceed the room’s capacity.</li>

 <li>The player cannot not flee to the exit-room.</li>

 <li>In rooms neighboring to the start-room, the player can always flee.</li>

 <li>After using a healing potion the player cannot flee at the next turn. So, if the the pot is used at turn <em><sub>t</sub></em>, at turn <em><sub>t</sub></em><sub>+</sub>1 it cannot flee (and since a creature can only do one action per turn, this implies that the player can’t flee in the same turn <em><sub>t </sub></em>either). At turn <em><sub>t</sub></em><sub>+</sub>2 will be able to flee.</li>

 <li>The player cannot flee while ’enraged’. Recall that the player enters such a state when whenever it uses a rage potion (this state lasts for 5 turns, including the turn when the potion is used). However, this restriction does not apply when the game is played in the Newbie-mode.</li>

</ol>

If the game is played in the Elite-mode things become harder: if the player is enraged while in a room (let’s call it room <em><sub>S</sub></em>) neighboring to the exit-room, the player will not be able to flee from <em><sub>S </sub></em>even after the rage effect has dissipated. Note that this does not mean that the player can never leave <em><sub>S</sub></em>. It can do so by using the ordinary <sub>MOVE </sub>command (which is only possible if it is no longer in combat). The logic for executing this action is to be implemented in the method <sub>Game</sub>.<sub>Flee</sub>(<em>c</em>), where <em>c </em>is the fleeing creature.

<h1>3        The Main Class</h1>

The class STVrogue.Program is the main class (the class with the <sub>Main </sub>method) that provides the console application for the game. When you start the application, it reads the game configuration from a file (configuration is explained in Section 2.5). It shows a welcome-screen, and the game begins. At each turn the game should display at least:

<ol>

 <li>The turn number.</li>

 <li>Player information: HP and KP.</li>

 <li>The id of the room the player is currently at, and those of connected rooms.</li>

 <li>Ids of monsters in the room.</li>

 <li>Items in the room.</li>

 <li>Items in the player’s bag.</li>

 <li>Avaialable actions for the player.</li>

</ol>

When the player does an action, print a message to the console informing the player of the effect of this action. When a monster in the current room does an action, also print similar message. Actions of monsters in other rooms should not be echoed to the console.

When the player wins or loses, print your ending message before exiting the game.

Right now the class STVrogue.Program contains a dummy implemetation just so that you can run it. Obviously, you should replace this dummy implementation with your own.

<h1>4        Important: Controlling Random</h1>

Like in many other games, some parts of STV Rogue are required to behave randomly (e.g. when generating dungeons, or when deciding monsters’ actions). When testing a program that behaves non-deterministically, the same test may yied different results when re-run with exactly the same inputs and configuration. Such a test is called ’flaky’ or ’unrepeatable’. Obviously we do not want to have flaky tests. To this end, you need to make it so that you can configure your implementation of STV Rogue to switch from using normal

random generators to using pseudo random generators when testing it<a href="#_ftn3" name="_ftnref3"><sup>[3]</sup></a>. Such a generator behaves deterministically when given the same seed. Check the class <sub>Utils</sub>.<sub>SomeUtils </sub>to obtain such a generator.

<h1>5        Your Tasks</h1>

Your tasks are listed below. All are mandatory, except Task 8. You should divide the work among your team members such that everyone has her/his fair share of testing. In fact, the author of a functionality should not be the only person to test the functionality due to her/his obvious bias.

<ol>

 <li>Move(<em>r</em>) and Creature.Attack(<em>f </em>) (0.5 pt).</li>

 <li>Dungeon(<em>shape</em>,<em>N</em>,<em>γ</em>) (1 pt). 3. Dungeon.SeedMonstersAndItems(<em>M</em>,<em>H</em>,<em>R</em>) (1 pt).</li>

 <li>Game(<em>conf </em>) (1 pt). 5. Game.Flee(<em>c</em>) (2 pt).</li>

 <li>Finishing the implementation of STV Rogue (2 pt).</li>

 <li>Test the rest of the game logic (1.2 pt).</li>

 <li>Optional: stronger testing of <sub>Flee</sub>(<em>c</em>) (1pt).</li>

 <li>Report (0.3 pt).</li>

</ol>

All produced tests should deliver 100% code coverage<a href="#_ftn4" name="_ftnref4"><sup>[4]</sup></a> on their test target (e.g., your tests on <sub>Flee</sub>(<em>c</em>) should give 100% coverage on this method). If you deliver less, you have to explain the reason in your Report (e.g. because the uncovered parts are unreachable, or simply because you run out of time).

Please document your test methods and in-code specifications/parameterized-tests. Write a comment describing what each test method tries to check. Inside the body of each in-code specification/parameterized test, write a comment explaining what correctness properties different parts of the specification try to capture.

The McCabe/Cyclometic metric of your method should give a rough indicator of the minimum number of test cases you would need to test it. The metric gives the number of

’linearly independent’ control paths in the method (check Wikipedia’s entry on Cyclometic complexity).

5.1    Test Creature.Move(<em>r</em>) and Creature.Attack(<em>f </em>) (0.5 pt)

To get you started in learning to do basic unit testing, test the above mentioned two methods to verify their correctness. The methods are already implemented, so you only need

<table width="329">

 <tbody>

  <tr>

   <td width="329">[ T e s t F i x t u r e ]p u b l i c c l a s s Test_Remainder { / / the t e s t s :[ TestCase ( 5 , 0 ) ][ TestCase ( 5 , 3 ) ][ TestCase ( 5 , <sub>− </sub>3 ) ][ TestCase ( <sub>−</sub>5 , <sub>−</sub>3)]. . ./ / the in <sub>−</sub>code spec . f o r % : p u b l i c void Spec_Remainder ( i n t x , i n t y ) {/ /    check     the      method<sub>−</sub>under<sub>−</sub>t e s t ‘ s     pre <sub>−</sub>c o n d i t i o n :if ( y    !=    0 )    {/ /   c a l l i n g    the     method<sub>−</sub>under<sub>− </sub>t e s t :i n t    r   =   x   %   y/ /   ( a )     check     the    method ‘ s     post <sub>−</sub>c o n d i t i o n :    r    i s   a c o r r e c t/ / reminder <sub>if </sub>i t i s e qual to x <sub>− </sub>d ∗ y , where d i s the / / r e s u l t of d i v i d i n g x with y :A s s s e r t . I s t r u e ( r   ==      x <sub>− </sub>( x     /   y )   ∗   y )    ;}else {/ / ( b )                  the                   method            should              throw              t h i s                      e x c e p t i o n when               i t s / /             pre <sub>−</sub>c o n d i t i o n   i s                     not                   s a t i s f i e d :A s s e r t . Throws &lt; DivideByZeroException &gt;( x         %   y )    ;}}}</td>

  </tr>

 </tbody>

</table>

<table width="675">

 <tbody>

  <tr>

   <td width="665">STV Project 2019/20, PART 1                                                                                                   Course Software Testing &amp; Verification,</td>

  </tr>

 </tbody>

</table>

Figure 2. <em>An example of how to write an NUnit test through an in-code specification. Let’s imagine we want to test C# remainder operator (%).</em>

to test them (and to fix them if you find bugs). Use NUnit Framework to write your tests.

5.2 Implement and test the constructor Dungeon(<em>shape</em>,<em>N</em>,<em>γ</em>) (1 pt)

The intended behavior of this constructor is informally specified in Section 2.2. Implement the constructor. Then, formalize its informal specification as an in-code specification and then use NUnit parameterized test to test the method. Figure 2 shows an example of how to do this. The class Utils.HelperPredicates contains some help pred-

icates you might find useful.

5.3 Implementation and test Dungeon.SeedMonstersAndItems(<em>M</em>,<em>H</em>,<em>R</em>) (1 pt)

The intended behavior of this constructor is informally specified in Section 2.2. Implement it and write in-code specification for the method. This time, formulate the in-code specification as NUnit <sub>Theory </sub>and then test that Theory.

Check NUnit Documentation: <a href="https://github.com/nunit/docs/wiki/NUnit-Documentation">https://github.com/nunit/ </a><a href="https://github.com/nunit/docs/wiki/NUnit-Documentation">docs/wiki/NUnit-Documentation</a><a href="https://github.com/nunit/docs/wiki/NUnit-Documentation">.</a> The entry about Theory should be listed under the category ’Attributes’. There is also an example of using Theory in the STV Rogue project itself.

5.4     Implementation and test the constructor Game(<em>conf </em>) (1 pt)

The intended behavior of this constructor is informally specified in Section 2.5. Implement the constructor and write in-code specification for this method. Formulate it as a parameterized test. This time, use NUnit combinatoric testing feature to generate tests for the constructor. Be mindful that full combinatoric test may blow up to thousands of test cases. You may want to consider pair-wise testing instead.

Check the entries on ’Combinatorial’ and ’Pairwise’ in NUnit documentation. There are also examples of these in the STV Rogue project itself.

5.5 Implementation and test the method Game.Flee(<em>c</em>) (2 pt)

The intended behavior of this method is informally specified in Section 2.5. This method has the most complicated logic. Implement and test it.

5.6 Finish the Implementation of STV Rogue (2 pt) Finish the implementation of STV Rogue to get a working game. Among other things, you will have to implement the method Game.Update(<em>cmd</em>) as well as finishing the main class Program.

5.7        Test the rest of the game logic (1.2 pt)

Finish the testing of the game logic (that is, of all classes in the STVrogue.GameLogic namespace).

5.8        Optional: stronger testing of Flee(<em>c</em>) (1pt)

The logic of the method <sub>Dungeon</sub>.<sub>Flee</sub>(<em>c</em>) is fairly complicaated. For such a method simple code coverage does not really reflect the adquacy of your tests as it cannot enforce path-level verification. Unfortunately there is no tool in the market that will let you do path coverage tracking. Let us try to compensate this by augmenting your existsing tests for <sub>Flee </sub>with combinatoric testing. Chapter 4 in Ammann &amp; Offutt’s book (Ch. 6 for 2nd Ed.) explains the main concepts. See also the slides from Week-2.

Identify the set of ’characteristics’ on which the behavior of <sub>Flee</sub>(<em>c</em>) depends on. These typically include the method parameters (there is only one: <em><sub>c</sub></em>), but also other aspects that are not formally listed as a parameter, e.g. the location of <em><sub>c</sub></em>, the use of healing potion, the difficulty-mode, etc.

Then, decide how you want to partition each characteristic into ’blocks’. E.g. <em><sub>c </sub></em>can be a monster or the player (so, we would have two blocks for <em><sub>c</sub></em>). The position of <em><sub>c </sub></em>can be distinguished between: a neighbor of the start-room, a neighbor of the exit-room, or other room. The used difficulty-mode can be distinguished between: the newbie-mode, the normalmode, or the elite-mode. And so on.

Translate your design into an NUnit combinatoric (or at least pair-wise) test using a parameterized test.

5.9          Report (0.3 pt, mandatory) Make a report containing the items listed below.

<ol>

 <li>The general statistics of your implementation:</li>

</ol>

<table width="216">

 <tbody>

  <tr>

   <td width="162"><em>N </em>=</td>

   <td width="15"> </td>

   <td width="39"> </td>

  </tr>

  <tr>

   <td width="162"><em>M </em><sub>= </sub>total #methods</td>

   <td width="15">:</td>

   <td width="39">…</td>

  </tr>

  <tr>

   <td width="162"><em>locs </em><sub>= </sub>total #lines of codes(*)</td>

   <td width="15">:</td>

   <td width="39">…</td>

  </tr>

  <tr>

   <td width="162"><em>locs<sub>avд </sub></em>= average #lines of codes(*)</td>

   <td width="15">:</td>

   <td width="39"><em>locs</em>/<em>N</em></td>

  </tr>

 </tbody>

</table>

total #classes                                          :      …

(*) exclude comments 2. Statistics of your unit-testing effort:

<table width="287">

 <tbody>

  <tr>

   <td width="287"> </td>

  </tr>

  <tr>

   <td width="287"><em>N</em><sup>′ </sup><sub>= </sub>#classes targeted by your unit-tests : … total coverage over <sub>GameLogic </sub>: … <em>T </em><sub>= </sub>#test cases (*)        : … <em>Tlocs </em><sub>= </sub>total #lines of codes (locs) of your unit-tests : …<em>Tlocs<sub>avд </sub></em>= average #unit-tests’ locs per target class             :           <em>Tlocs</em>/<em>N</em><sup>′</sup><em>E </em><sub>= </sub>total time spent on writing tests   :                       … <em>E<sub>avд </sub></em>= average effort per target class       :                       <em>E</em>/<em>N</em><sup>′ </sup>total #<sub>bugs </sub>found by testing           :                       …</td>

  </tr>

  <tr>

   <td width="287">Statistics of some selected targets</td>

  </tr>

  <tr>

   <td width="287">Dungeon(<em>shape</em>, <em>N</em>, <em>γ</em>)mcCabe metric         : … # test-cases (*)            : … coverage                    : …Dungeon.SeedMonstersAndItems(<em>M</em>, <em>H</em>, <em>R</em>)mcCabe metric         : … # test-cases (*)            : … coverage                    : …Game(<em>conf </em>)mcCabe metric         : … # test-cases (*)            : … coverage                    : …Game.Flee(<em>c</em>)mcCabe metric         : … # test-cases (*)            : … coverage                    : …Game.Update(<em>cmd</em>)mcCabe metric         : … # test-cases (*)            : … coverage                    : …</td>

  </tr>

 </tbody>

</table>

Global Statistics

(*) We will define the ’number of test-cases’ as the number of tests that NUnit reports.

<ol start="3">

 <li>Explanation: if your coverage is below 100%, mention why you failed to get it to 100.</li>

 <li>If you do the Optional Task (Section 5.8), describe your chosen set of characteristics and how they are divided into blocks. Describe your chosen approach of combinatoric testing, and how this is translated to NUnit parameterized test.</li>

 <li>Specify how the work is distributed among your team members, in terms of who is doing what, and the percentage of the total team effort that each person shoulders.</li>

</ol>

<a href="#_ftnref1" name="_ftn1">[1]</a> <a href="https://azureforeducation.microsoft.com/devtools">https://azureforeducation.microsoft.com/devtools </a><a href="https://nunit.org/"><sup>4</sup></a><a href="https://nunit.org/">https://nunit.org/</a>

<a href="#_ftnref2" name="_ftn2">[2]</a> For a larger game with a more complex it would make sense to introduce more decomposition. STV Rogue is not that complex though; so, to favor simplicity I will keep most of the logic centralized in the class <sub>Game</sub>.

<a href="#_ftnref3" name="_ftn3">[3]</a> Well, a ’normal’ random generator is typically also a pseudo random generator. It is just that its seed is not fixed, e.g. it is based on the system time. For STV Rogue, you can alternatively only use pseuodo random generators. It is not something you should do when implementing a real game, but in this project implementing real randomness is not our focus.

<a href="#_ftnref4" name="_ftn4">[4]</a> Visual Studio tracks both line coverage and block coverage. The concept of ’block’ coverage is explained in one of the lectures. Rider uses a different concept, namely statement coverage. It means that Rider can tell you which statements are covered or otherwise. This is slightly more coarse grained than block coverage. E.g. if you have a statement <sub>if</sub>(<em>p </em>||<em>q</em>) <em>x</em><sub>++</sub>, Rider can tell you whether or not you have executed the <em>x</em><sub>++ </sub>in the then-branch, but it cannot tell whether you have explored all the possibilities for enabling its guard (either due to <em>p </em>is true, or <em>q </em>is true), because techically a guard is an expression rather than a statement.