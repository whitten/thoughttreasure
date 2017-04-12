
ThoughtTreasure: A natural language/commonsense platform
by Erik T. Mueller

August 22, 2003
Abstract
This paper provides an overview of ThoughtTreasure, a comprehensive platform for natural language processing and commonsense reasoning. ThoughtTreasure contains on the order of 100,000 pieces of common sense associated with 55,000 English and French words and phrases. We describe the ThoughtTreasure commonsense knowledge base and architecture for natural language processing. We then discuss how commonsense knowledge is represented in ThoughtTreasure using multiple schemes including logic, finite automata, and grids. We then present applications of ThoughtTreasure, including story understanding by building and maintaining a simulation of the states and events described in a story.
1. Introduction

Under development since 1994, ThoughtTreasure is a comprehensive platform for natural language processing and commonsense reasoning (Mueller, 1998). ThoughtTreasure contains pieces of common sense such as:

    A hotel room has a bed, night table, minibar, ...
    A person has zero or one husbands.
    A play lasts about two hours.
    Cassette means cassette tape or cassette recorder.
    Gold hair is called blond hair.
    One hangs up at the end of a phone call.
    People have fingernails.
    Rough synonyms for food are foodstuffs, groceries, chow, grub, ...
    Soda is a drink.
    Someone who is 16 years old is called a teenager.
    The sky is blue. 

Human commonsense knowledge is large, probably on the order of 1,000,000,000 bits (Landauer, 1986). So far, ThoughtTreasure's knowledge is much smaller, on the order of 10,000,000 bits. But even a small amount of common sense can be used to improve applications. For example, ThoughtTreasure's commonsense and natural language capabilities have been used to build a calendar program that fills in missing information and points out potential problems such as scheduling a dinner appointment for 6 am or taking a vegetarian to a steakhouse (Mueller, 2000).

This paper provides an overview of ThoughtTreasure. We first present the ThoughtTreasure knowledge base and architecture for natural language processing. We then discuss how knowledge is entered into the ThoughtTreasure knowledge base. We then describe the multiple representation schemes used to represent commonsense knowledge in ThoughtTreasure. We then describe two major application areas of ThoughtTreasure: simple factual question answering and more general natural language understanding. We conclude with status and future work.
2. The ThoughtTreasure knowledge base and architecture

The ThoughtTreasure knowledge base contains 25,000 concepts organized into a hierarchy. For example, Evian is a type of flat-water, which is a type of drinking-water, which is a type of beverage, which is a type of food, and so on. Each concept has one or more English and French approximate synonyms, for a total of 55,000 words and phrases. For example, associated with the food concept are the English words food and foodstuffs and the French words aliment and nourriture (and others). ThoughtTreasure contains 50,000 assertions, such as the fact that a green-pea is a seed-vegetable, a green-pea is green, a green-pea is part of a pod-of-peas, and a pod-of-peas is found in a typical grocery store.

ThoughtTreasure contains about 100 scripts, or representations of typical situations or activities such as eating at a restaurant or attending a birthday party (Mueller, 1999; Schank & Abelson, 1977). Each script contains information about the events that make up the script, roles played by people and physical objects, entry conditions, results of performing the script, personal goals satisfied by the script, emotions associated with the script, where the script is performed, how long it takes to perform the script, how often the script is performed, and the cost of the script.

ThoughtTreasure was inspired by Cyc (Lenat, 1995) and is similar to Cyc in that it has both natural language and commonsense components. Some differences are as follows: Whereas ThoughtTreasure uses multiple representations including logic, finite automata, grids, and scripts, Cyc mostly uses logic. Most assertions in ThoughtTreasure are represented in a simple relational-database-like format that is easier for applications to use than the arbitrary first-order predicate calculus assertions of Cyc.

ThoughtTreasure's lexicon is similar to WordNet (Fellbaum, 1998) but differs in several important respects. WordNet is a lexical, rather than conceptual, database. World knowledge is explicitly excluded from WordNet (pp. 5-6). Typical object properties, such as the fact that the sky is blue, are not included in WordNet. Scripts are not included in WordNet since concepts such as eating at a restaurant and going to a birthday party are not lexicalized in English. WordNet contains separate databases for nouns, verbs, adjectives, and adverbs. Unlike in ThoughtTreasure there is no connection between the noun happiness and the adjective happy. (Derivational morphology relations between nouns and verbs have been added in WordNet 2.0.) Unlike ThoughtTreasure, WordNet lacks a root concept and top-level ontology for organizing all concepts. ThoughtTreasure contains more detailed information about argument structure.

The ThoughtTreasure architecture is implemented as 80,000 lines of code and consists of:

    the text agency, containing text agents for recognizing words, phrases, names, and phone numbers, and mechanisms for learning new words and inflections,
    the syntactic component, containing a syntactic parser, base rules, and filters,
    the semantic component, containing a semantic parser for producing a surface-level understanding of a sentence, a natural language generator, and an anaphoric parser for resolving anaphoric entities such as pronouns,
    the planning agency, containing planning agents for achieving goals on behalf of simulated actors, and
    the understanding agency, containing understanding agents for producing a more detailed understanding of a discourse. 

3. Entering knowledge into the ThoughtTreasure knowledge base

A compact notation is used to speed entry of knowledge into the ThoughtTreasure knowledge base. Here is how the notation is used to define some knowledge about subatomic particles:

==hadron.z//strongly#B interacting# particle*.z/hadron.My/
===baryon.z//qqq.tz/
====unflavored#A baryon*.z//|strangeness-of=0u|charm-of=0u|bottomness-of=0u|
=====nucleon.z/fermion/|spin-of=.5u|isospin-of=.5u|
======neutron.z//n.tz/n# baryon*.z/neutron.My/|electric-charge-of=0u|
baryon-number-of=1u|part-of¤up-quark,down-quark,down-quark|
======antineutron.z//antineutron.My/|electric-charge-of=0u|baryon-number-of=-1u|antiparticle-of=neutron|
======proton.z//p.tz/proton.My/|electric-charge-of=1u|baryon-number-of=1u|
part-of¤up-quark,up-quark,down-quark|
======antiproton.z//antiproton.My/|electric-charge-of=-1u|baryon-number-of=-1u|
antiparticle-of=proton|

The level of indentation shows the hierarchical structure: a baryon is a type of hadron; a neutron, antineutron, proton, and antiproton are all a type of nucleon. Words for concepts are defined at the same time concepts are defined. The leftmost word or phrase is also used as the name of the concept. Thus hadron is a concept, and words for this concept are hadron in English, strongly interacting particle in English, and hadron in French. If there is a conflict, a concept name may be defined without defining a word:

==particle-hadron//hadron.z/

Single characters are used to represent features. For example, lower case z refers to English. Lower case y refers to French. The feature character A refers to adjective and B refers to adverb. Lexical entries are assumed to be nouns unless otherwise specified.

The second element of each line between the slashes is used to indicate additional parents. For example, a nucleon is a fermion as well as an unflavored-baryon:

====unflavored#A baryon*.z//|strangeness-of=0u|charm-of=0u|bottomness-of=0u|
=====nucleon.z/fermion/|spin-of=.5u|isospin-of=.5u|

Thus words, phrases, and concepts, and hierarchical structure may all be defined at the same time.

Assertions are also easily added. For example, assertions on a proton may be defined: a proton's electric charge is 1, its baryon number is 1, and it consists of two up quarks and a down quark:

======proton.z//p.tz/proton.My/|electric-charge-of=1u|baryon-number-of=1u|
part-of¤up-quark,up-quark,down-quark|

Single character feature codes are also used to represent the argument structure of verbs. For example, é indicates that a verb takes a direct object:

call.Véz/

V = verb
é = takes direct object
z = English

Each word in a phrase is followed by either a star or number sign, which indicates whether that word is inflected. One says I call up but he calls up:

call* ø up#R.VÀéz/

* = inflects
# = does not inflect
R = preposition
ø = location of direct object
V = verb
À = Americanism

ø indicates the location of the direct object: one says call him up and not call up him.

Some verbs take prepositional phrase arguments, such as lie on as in I was lying on the couch. A plus sign after the preposition indicates it is required; an underscore indicates it is optional:

lie* on+.Vz/
lie* down#R on_.Vz/

+ = required preposition
_ = optional preposition

There are a number of other single character codes for encoding verbs:

Verb features

ú = subject assigned to slot 2
ü = subject assigned to slot 3
è = object assigned to slot 1
é = object assigned to slot 2
ë = object assigned to slot 3
÷ = indicative "(that) he goes"
O = subjunctive "(that) he go"
Ï = infinitive "(for him) to go"
± = present participle "(him) going"

4. Representation of commonsense knowledge in ThoughtTreasure

ThoughtTreasure adopts the view of Minsky (1986) that there is no single, "right" representation for all tasks. Thus ThoughtTreasure uses multiple representation schemes:

    grids for stereotypical settings,
    finite automata/code for rules of thumb, device behavior, and mental processes (similar, but typically more complex than, the x-schemas of the KARMA model of Narayanan, 1997), and
    logical assertions for encyclopedic facts and linguistic knowledge. 

4.1 Stereotypical settings

Settings such as the ground floor of a theater are represented in ThoughtTreasure as grids (similar to ASCII art):

==Opera-Comique-Rez/floor/|col-distance-of=.3m|row-distance-of=.6m|level-of=0u|
orientation-of=south|part-of=Opera-Comique|polity-of=2arr|has-ceiling|
@19980201:naÐ|GS=
 wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwww
 w      wuu2uuuw                               wuu3uuuwuuuuuuw
 w      wuuuuuuw                               wuuuuuuwuuuuuuw
 w      wuuuuuuw                               wuuuuuuwuuuuuuw
 wCC    wuuuuuuw                               wuuuuuuwuuuuCCw
 w      wC     w                               w     Cw      w
 w      w      wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwww      w      p
 w      w      CC X                           CC      w      w
 p      p      p                               p      p      P
 p      p      ttt                           ttt      p      P
 p      p      ttt     A                     ttt      p      P
 w      w      tttttttRTtttttttttttttttttttttttt      w      w
 w      w      ttttttttttttttttttttttttttttttttt      w      w
 w      w              B                              w      w
 p      p                                             p      P
 p      p                                             p      P
 p      p                                             p      P1
 w      w                                             w      w
 wwwwwwww                                             w      p
5Q      w                                             w      w
 Q      w                                             wuuuuuuw
 wuuuuuuw                   c  c  c                   wuuuuuuw
 wCu4uuCwC     CMMMMC     CMMMMMMMMMC     CMMMMC     CwCuuuuCw
 wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwww
.
p:door
t:counter
u:stairs-up
w:wall
A:employee-side-of-counter
B:customer-side-of-counter
C:column
M:mirror
P:revolving-door
Q:stage-entrance
R:&Opera-Comique-cash-register
T:&Opera-Comique-ticket-box
X.tpCw:&Opera-Comique-box-office
1:&Opera-Comique-Entrance1
2:&Opera-Comique-0To1A
3:&Opera-Comique-0To1B
4:&Opera-Comique-0To1Stage
5:&Opera-Comique-Stage-Entrance
.
|

This grid can be interpreted literally as a particular ground floor of a theater in Paris, or used to guess how any theater ground floor might be arranged.

The definition consists of the name of the grid and then several assertions about the grid: The distance between each successive grid element horizontally is .3 meters; the distance between each grid element vertically is .6 meters; this is the ground floor, level 0; its orientation is south; it is part of the Opera Comique building; it is located in the 2nd arrondissement of Paris; and it has a ceiling.

The state of this grid is defined as of February 1, 1998. Each character is defined at the bottom: The character w indicates a new instance of an object of type wall. p defines an object of type door. On the left are some doors leading into the middle lobby area. When this definition is read in, contiguous instances of the same letter are interpreted as single objects. Thus the three ps in a row on the left constitute a single door. The next three ps constitute another door. t indicates an object of type counter. In the middle is a large contiguous ticket counter. Once the objects are defined, they can potentially be moved in the grid.

A wormhole enables definition of a collection of connected grids. The grid for the ground floor of the theater is defined, the grid for the first floor of the theater is defined, and then those grids are connected using wormholes. At the top is a 2. In the legend, 2 is defined as a wormhole that connects the ground floor to the first floor. The first floor, defined by another grid, also contains that same wormhole. Thus if an actor wishes to get from the ground floor to the first floor, the actor would go through that wormhole.

Grids make it easy to code new stereotypical settings. They also allow certain 2-d spatial operations to be performed efficiently. For example, a grid representation lends itself to finding paths through a space containing many obstacles. Paths are used in ThoughtTreasure to make inferences about whether it is possible to walk from one location to another, or whether two people can hear each other.

ThoughtTreasure can use grids and wormholes to plan a trip such as one starting from a street in Paris, to New York:

    walk to subway stop in a street grid,
    take subway on a subway grid,
    board plane in an airport grid, and
    fly along Atlantic Ocean in an earth grid. 

ThoughtTreasure contains the following grids:

    Living spaces: city-apartment1, city-apartment2, small-apartment-building-floor, small-apartment-building-ground-floor, large-apartment-building-ground-floor, country-house-ground-floor, hotel-ground-floor, hotel-room-and-floor.
    Eating/food places: restaurant, bar, grocery-store.
    Entertainment areas: theater-ground-floor, theater-hall, TV-studio.
    Outdoor areas: city-street1, city-street2, city-street-and-park, country-area.
    Transportation areas: subway-ticket-area, subway-platform1, subway-platform2, subway-platform3, subway-tracks, airport1, airport2, highway-map1, highway-map2, highway-map3, earth1. 

4.2 Rules of thumb

Rules of thumb such as the following are represented procedurally in ThoughtTreasure:

    Buy tickets at the box office.
    Look both ways before crossing the street.
    Turn the shower on before getting in.
    Don't wear the same clothes two days in a row.
    Don't leave home without your wallet.
    At the end of a phone conversation, say bye and hang up. 

Consider the first example, buy tickets at the box office. This is represented in the following planning agent for purchasing a ticket P on behalf of actor A:

purchase-ticket(A, P) :-
    dress(A, purchase-ticket),
    RETRIEVE building-of(P, BLDG);
      near-reachable(A, BLDG),
    near-reachable(A, FINDO(box-office)),
    near-reachable(A, FINDO(customer-side-of-counter)),
2:  interjection-of-greeting(A, B = FINDO(human NEAR employee-side-of-counter)),
    WAIT FOR may-I-help-you(B, A)
      OR WAIT 10 seconds AND GOTO 2,
5:  request(A, B, P),
6:  WAIT FOR I-am-sorry(B) AND GOTO 13
      OR WAIT FOR describe(B, A, TKT = ticket) AND GOTO 8
      OR WAIT 20 seconds AND GOTO 5,
8:  WAIT FOR propose-transaction(B, A, TKT, PRC = currency),
    IF TKT and PRC are OK accept(A, B) AND GOTO 10
    ELSE decline(A, B) AND GOTO 6,
10: pay-in-person(A, B, PRC),
    receive-from(A, B, TKT) ON FAILURE GOTO 13,
    ASSERT owner-of(TKT, A),
    post-sequence(A, B),
    SUCCESS,
13: post-sequence(A, B),
    FAILURE.

A planning agent is an arbitrary finite automaton that can loop around, wait for various conditions, and so forth. This planning agent works as follows: First a subgoal is activated for A to get dressed for purchasing a ticket. That invokes another planning agent for dressing, which causes A to select clothes that are appropriate to purchasing a ticket and then to get dressed. Once that subgoal is achieved then another subgoal is activated for A to be near the box office. (Of course there could be other plans for purchasing a ticket. This particular planning agent only knows about the plan of going to the box office to purchase the ticket.)

A goes to the box office. Then A goes to the customer side of the counter in the box office. FINDO (find object) finds a nearby object of a particular type and returns it. That is, once one gets to the box office, one can then locate the customer side of the counter. Then state 2 of the finite automaton is as follows: A says hello to the human who is located on the employee side of the counter. Then the planning agent waits until that person, called B, says may I help you, or any other lexical item defined for the may-I-help-you concept. Alternatively, it waits 10 seconds and then goes back to state 2. This planning agent will say hello every 10 seconds until it gets some service.

Then once B says may I help you, A requests the ticket and then the planning agent waits for an I-am-sorry response such as We're all sold out in which case this finite automaton goes to state 13 and A then says goodbye to B and this subgoal fails. Or, the planning agent waits for B to describe a particular ticket to A in which case it goes to state 8. The agent then waits for B to specify a price for the ticket. A then has to decide whether the date, time, and location of the ticket are acceptable. If they are acceptable, A accepts and the planning agent goes to state 10.

The subgoal for A to pay B the agreed upon amount is then activated. A planning agent is activated for paying, using various techniques such as cash, check, and credit card. Once it is paid, the agent activates the subgoal for A to receive the ticket from B. The receive planning agent involves extending a hand to receive the ticket from the other person and then grabbing it and putting it away. Then the fact that this ticket is now owned by A is asserted. A says goodbye to B and then this planning agent terminates with success. So this subgoal would succeed in that case. If the ticket is not received then the subgoal fails.

Corresponding to the planning agent of the customer is a planning agent for the employee working the box office:

work-box-office(B, F) :-
     dress(B, work-box-office),
     near-reachable(B, F),
     TKTBOX = FINDO(ticket-box);
       near-reachable(B, FINDO(employee-side-of-counter)),
/* HANDLE NEXT CUSTOMER */
100: WAIT FOR attend(A = human, B) OR
       pre-sequence(A = human, B),
     may-I-help-you(B, A),
/* HANDLE NEXT REQUEST OF CUSTOMER */
103: WAIT FOR request(A, B, R) AND GOTO 104
       OR WAIT FOR post-sequence(A, B) AND GOTO 110,
104: IF R ISA tod {
       current-time-sentence(B, A) ON COMPLETION GOTO 103
     } ELSE IF R ISA performance {
       GOTO 105
     } ELSE {
       interjection-of-noncomprehension(B, A)
       ON COMPLETION GOTO 103
     }
105: find next available ticket TKT in TKTBOX for R;
       IF none {
         I-am-sorry(B, A) ON COMPLETION GOTO 103
       } ELSE {
         describe(B, A, TKT) ON COMPLETION GOTO 106
       },
106: propose-transaction(B, A, TKT, TKT.price),
     WAIT FOR accept(A, B) AND GOTO 108
       OR WAIT FOR decline(A, B) AND GOTO 105
       OR WAIT 10 seconds AND GOTO 105,
108: collect-payment(B, A, TKT.price, FINDO(cash-register)),
109: hand-to(B, A, TKT),
110: post-sequence(B, A) ON COMPLETION GOTO 100.

B dresses appropriately for the job and goes to the box office and goes to the employee side of the counter, waits for a customer A to look at B or say hello. Then B says may I help you and waits for a request and upon obtaining a request, tries to handle that request. If the request is What time is it?, B will answer with the time and the planning agent will go back to state 103 to wait for another request.

If the request is a performance, then B will try to find the next available ticket for that performance. If there are none left, B will say I'm sorry, we don't have any. Otherwise B will describe the attributes of that ticket and its price, and either wait for A to accept or decline. If A accepts, B collects the payment and hands A the ticket. If the customer declines, the planning agent goes back to state 105 to suggest another possible ticket. If the request is not understood, B says What? to A and another request is awaited. When A says goodbye, B says goodbye and the planning agent goes back to state 100 to wait for other customers.

Finite automata provide an efficient way of simulating the behavior of actors, useful in applications such as interactive fiction or controlling a robot.

ThoughtTreasure contains the following planning agents:

    Grasper: Grasp, Release, Holding, Connect, ConnectedTo, Rub, Pour, SwitchX, FlipTo, KnobPosition, GestureHere, HandTo, ReceiveFrom.
    Container: ActionOpen, ActionClose, Open, Closed, Inside.
    Movement: MoveTo, MoveObject, SmallContainedObjectMove, HeldObjectMove, GrasperMove, ActorMove, LargeContainerMove.
    Transportation: NearAudible, NearReachable, NearGraspable, GridWalk, Sitting, Standing, Lying, Sit, Stand, Lie, Warp, Drive, GridDriveCar, MotorVehicleOn, MotorVehicleOff, Stay, Pack, Unpack.
    Communication: Mtrans, ObtainPermission, HandleProposal, Converse, Call, HandleCall, OffHook, OnHook, PickUp, HangUp, Dial.
    Money: PayInPerson, PayCash, PayByCard, PayByCheck, CollectPayment, and CollectCash.
    Interpersonal relations: MaintainFriends Appointment.
    Entertainment: AttendPerformance, WorkBoxOffice, PurchaseTicket, WatchTV, TVSetOn, TVSetOff.
    Personal: Sleep, Shower, Dress, PutOn, Wearing, Strip, TakeOff. 

ThoughtTreasure also contains declarative versions of planning agents or scripts (Mueller, 1999).
4.3 Behavior of electronic devices

Knowledge about the behavior of electronic devices such as:

    When an idle phone line goes off hook, the phone gives a dialtone.
    When a ringing phone goes off hook, the phone stops ringing and is connected to the caller. 

is represented by the phone device agent:

H = FINDP(phone-handset, T)
IF condition(T, W) and W < 0 { /* T broken */
  ASSERT idle(T)
} ELSE IF idle(T) {
  IF off-hook(H) ASSERT dialtone(T)
} ELSE IF dialtone(T) {
  IF on-hook(H) ASSERT idle(T) ...  
} ELSE IF ringing(T, CLG = phone) {
  IF off-hook(H) {
    ASSERT voice-connection(CLG, T)
    ASSERT voice-connection(T, CLG)
  }
...

ThoughtTreasure contains the following device agents:

    Car: off, on, ignition.
    TV: off, on, channel, antenna, plug.
    Phone: hook, state, handset, dial, phone number, connection.
    Shower: off, on, faucet, shower head, washing hair, shampoo. 

Human mental processes are also represented procedurally. If someone learns of the success of a friend, the emotion understanding agent will generate a positive emotion for that person. This agent also causes emotions to decay over time.
4.4 Encyclopedic facts

As mentioned above, encyclopedic facts such as:

    Washington is the capital of the U.S.
    Mushrooms have stems.
    Peas are green.
    747s are made by Boeing. 

are represented as assertions:

=====747.¹®z//|product-of¤Boeing|
======747#®-400#®.®z//|travel-crew-of=2u|
travel-passengers-of=412¯509u|
travel-cargo-capacity-of=6030lbs|
length-of=211ft|width-of=231.9ft|height-of=63.5ft|
weight-of=870000lbs|travel-max-speed-of=630mph|
travel-max-distance-of=8380mi|
@198805|create¤Boeing|

4.5 Common linguistic knowledge

Some linguistic knowledge is hardcoded, such as the fact that you refers to the listener of the conversation. The fact that a question of the form Is NOUN ADJECTIVE? requires a response of yes or no is coded in the Yes-No question understanding agent. The fact that gold hair is called blond hair is a selectional restriction, which is represented as an assertion:

==yellow#-orange*.NAz//jaune*-orange*.My/
|frequency-of=587.9ang|
===gold.NAz//couleur* d#R'or#NM.Fy/
====blond.Az//blond.Ay/|r1=hair|

The fact that tonight refers to the night of the current day is represented as an assertion and used by the time text agent to parse temporal expressions, and the generator to generate temporal expressions:

==relative-day-and-part-of-the-day/temporal/
|unit1-of=day|unit2-of=tod|
===last#A night*.Éz//
|min-value1-of=-1u|max-value1-of=-1u|
min-value2-of=NUMBER:tod:2100|
max-value2-of=NUMBER:tod:2400|
===tonight.Éz//ce#D soir#.ÉMy/
|min-value1-of=0u|max-value1-of=0u|
min-value2-of=NUMBER:tod:2100|
max-value2-of=NUMBER:tod:2400|

The fact that an X-phile is someone who likes X is represented as an assertion and used by ThoughtTreasure's word formation mechanisms to learn new words:

==phile.»z//|lhs-pos-of=Adjective|
rhs-pos-of=Noun|rhs-class-of=human|
[rhs-assertion-of phile [like rhs-obj lhs-obj]]|

ThoughtTreasure currently contains 152 English affixes associated with 38 derivational rules.
5. Simple factual question answering with ThoughtTreasure

We now proceed to describe some applications of ThoughtTreasure. The first major application area is simple factual question answering, or answering natural language questions about assertions stored in the knowledge base. Here is a sample Java program:

import com.signiform.tt.*;
public class Example {
public static void main(String args[])
{
  try {
    TTConnection tt = new TTConnection("somehost");
    System.out.println(
      tt.chatterbot(String.valueOf(TT.F_ENGLISH), "Who created Bugs Bunny?"));
    tt.close();
  } catch (Exception e) {
  }
}
}

The ThoughtTreasure Java-based client API is first imported. In the program's main method, a connection is created to a ThoughtTreasure server. The chatterbot method is then invoked with a question in English, and the results are printed. Finally the connection is closed. When the program is run, it prints:

Tex Avery created him.

ThoughtTreasure produces this answer as follows: First, text agents recognize words and phrases in the text Who created Bugs Bunny? The word Who is recognized, which can be a pronoun or a noun as in the rock group the Who. The word created can be a preterit or a past participle. Bugs can be the plural of bug or the singular of the first name Bugs. In addition, the phrase Bugs Bunny is recognized as a singular noun and bunny is recognized as a single word noun:

[H <Who.Hz:who 0-3:<Who>]
[N <Who.SNz 0-3:<Who>]
[V <created.iVz:create 4-11:<created>]
[V <created.dVz:create 4-11:<created>]
[N <Bugs.PNz:bug 12-16:<Bugs>]
[N <Bugs.SNz¸ 12-16:<Bugs>]
[N <Bugs Bunny.SMNz<?\n 12-23:<Bugs Bunny?\n>]
[N <Bunny.SNz:bunny<?\n 17-23:<Bunny?\n>]

The syntactic parser then takes the parse nodes produced by the text agents and builds parse trees. Among these are two that differ as to whether Who is a noun or pronoun:

[Z [X [H <Who.Hz:who>]]
 [W [W [V <created.iVz:create>]]
    [X [N <Bugs Bunny.SMNz>]]]]

[Z [X [N <Who.SNz>]]
 [W [W [V <created.iVz:create>]]
    [X [N <Bugs Bunny.SMNz>]]]]

The semantic parser takes the parse trees produced by the syntactic parser and converts them into logical assertions (similar to the quasilogical form of Alshawi, 1992). Four interpretations are produced: Who created Bugs Bunny, the character?, Who created the Bugs Bunny cartoon?, Did the rock group the Who create the Bugs Bunny character?, and Did the rock group the Who create the Bugs Bunny cartoon?:

1.0:[create human-interrogative-pronoun Bugs-Bunny]
0.9:[create human-interrogative-pronoun Bugs-Bunny-cartoon]
0.6:[create rock-group-the-Who Bugs-Bunny] ?
0.5:[create rock-group-the-Who Bugs-Bunny-cartoon] ?

The question understanding agents then run on each of these interpretations. One agent queries the knowledge base for a person who created Bugs Bunny and the agent finds that Tex Avery created Bugs Bunny. The agent assigns a make sense rating of 1.0 to the first interpretation, since an answer was found:

1.0:[create Tex-Avery Bugs-Bunny]

Another question agent queries the knowledge base for whether the rock group the Who created Bugs Bunny and determines that it did not. It returns the answer, No, the rock group the Who did not create Bugs Bunny and assigns a low make sense rating to that interpretation of the question:

.11:[not [create rock-group-the-Who Bugs-Bunny]]
.10:[not [create rock-group-the-Who Bugs-Bunny-cartoon]]

After the question answering agents have run on each of the interpretations, the one with the highest make sense rating is selected as the correct interpretation, and the corresponding answer is returned:

"Tex Avery created him."
("No, the Who did not create him.")
("No, the Who did not create it.")

From Java and other languages using the ThoughtTreasure server protocol, it is also possible to:

    query whether one concept is an instance of another concept,
    query whether one concept is a part of another concept,
    retrieve the parents, children, ancestors, and descendants of a concept,
    retrieve an arbitrary assertion from the knowledge base,
    convert a natural language phrase into a ThoughtTreasure concept,
    convert a ThoughtTreasure concept into natural language,
    perform part-of-speech tagging of text,
    generate syntactic parse trees of text, and
    generate semantic parses of text. 

6. Natural language understanding with ThoughtTreasure

The second major application area of ThoughtTreasure is general natural language understanding. Getting a computer to understand natural language text is a very difficult problem, because natural language is highly ambiguous. Consider the text I am. This could be a pronoun followed by a form of the verb be, or the Roman numeral I followed by the abbreviation for before noon. Pierre could be a human name or the capital of South Dakota. Even short sentences can have hundreds of parses! A computer has no way of knowing which one is right. Or does it? Could not one have the computer keep interpretations that make sense, and discard those that do not? This is what ThoughtTreasure's understanding agents attempt to do. Understanding agents decide whether a given input concept makes sense given the current context. Each understanding agent returns a make sense rating that indicates how much an input concept makes sense along with reasons why that concept makes or does not make sense.
6.1 Understanding emotions

Here is an example of how ThoughtTreasure's understanding agents work to understand a text involving emotions and interpersonal relations. First, we input the text Jacques is an enemy of François. The friend understanding agent accepts and stores that assertion:

> Jacques is an enemy of François.
[enemy-of Francois Jacques]

We then input that Jacques hates François. The friend understanding agent then returns that this makes sense because hating is consistent with being an enemy of, and that is generated in English as Right, he's an enemy of François:

> He hates François.
[like-human Jacques Francois NUMBER:u:-0.55]
  ----UA_Friend----
  makes sense because
  [enemy-of Francois Jacques]
Right, he is an enemy of François.

Then we input He uses tu with François. Now, this does not make sense to the friend understanding agent because in French enemies do not typically use tu with each other unless they are being very disrespectful. So ThoughtTreasure generates But I thought that he was an enemy of François. Still it is the only interpretation that ThoughtTreasure finds, so it is accepted:

> He uses tu with François.
[tutoyer Jacques Francois]
  ----UA_Friend----
  does not make sense because
  [enemy-of Francois Jacques]
But I thought that he was an enemy of François.

Then another input is entered, which is again stored by the friend agent:

> Lionel is an enemy of Jacques.
[enemy-of Jacques Lionel]

Then we input that Lionel uses tu with Jacques and this makes sense to the understanding agent because they both went to the École nationale d'administration, and people from that school use tu with each other:

> He uses tu with Jacques.
[tutoyer Lionel Jacques]
  ----UA_Friend----
  makes sense because
  [diploma-of Lionel promotion-Stendhal na ENA]
  [diploma-of Jacques promotion-Vauban na ENA]
Right, Lionel holds a "promotion Stendhal" from the "École nationale
d'administration" and he holds a "promotion Vauban" from the "École
nationale d'administration".

Then we input that Jacques succeeded at being elected President of France, which is stored:

> Jacques succeeded at being elected President of France.
[succeeded-goal Jacques [President-of France Jacques]]

Then the fact that Jacques is happy is input. Now, this makes sense to the emotion understanding agent because a positive emotion had already been generated earlier when that goal succeeded:

> He is happy.
[happiness Jacques]
  ----UA_Emotion----
  makes sense because
  [succeeded-goal Jacques [President-of France Jacques]]
Right, he succeeds at being the president of France.

Then Lionel is resentful toward Jacques is entered, which makes sense to the emotion understanding agent because a goal succeeded for Jacques and Jacques is an enemy of Lionel:

> Lionel is resentful toward Jacques.
[resentment Lionel Jacques]
  ----UA_Emotion----
  makes sense because
  [succeeded-goal Jacques [President-of France Jacques]]
  [enemy-of Jacques Lionel]
Right, he succeeds at being the president of France and Lionel is his
enemy.

Then we input François is happy for Jacques and this makes sense to the extent that Jacques experienced a goal outcome, but does not make sense because Jacques is an enemy of François.

> François is happy for Jacques.
[happy-for Francois Jacques]
  ----UA_Emotion----
  makes sense because
  [succeeded-goal Jacques [President-of France Jacques]]
  does not make sense because
  [enemy-of Francois Jacques]
True, he succeeds at being the president of France.
But I thought that he was an enemy of François.

The emotion understanding agent makes reference to a number of emotion concepts. ThoughtTreasure contains 650 emotion words in French and English attached to 83 emotion concepts, organized into positive and negative emotions, amusement, contentment, ecstasy, enjoyment, gloating, gratitude, gratification, happiness, and so on.
6.2 Story understanding by simulation

The output of the semantic parser consists of basic assertions that are fairly close to the input text. Jacques is an enemy of François becomes enemy-of Jacques François. This is not deep semantics. If the computer could instead construct a model or simulation of what is described in the input text and keep that simulation in sync with that input text, then the computer would be able to understand at a detailed level. It could easily answer questions by examining the simulation.

We propose that to build a computer that understands stories, we should build a computer that can construct simulations of the states and events described in the story. Further, we may modularize the problem by having different agents work on creating and maintaining different parts of the simulation.

We can think of the story understanding process as lining up cylinders of a combination lock. We can think of each planning agent as a cylinder that is in a given position. For example, the work-box-office planning agent discussed above could be in the state where it is handling a certain customer's request, say state 3. What the understanding agents try to do is to line up those planning agent cylinders given each new input:

           PA1 PA2 PA3
          +---+---+---+
          |  8|  8|  8|
          |  9|  9|  9|
    state-|  0|  0|  0|
          |  1|  1|  1|
          |  2|  2|  2|
          +---+---+---+
               |
               | UAs
               |
               V
          +---+---+---+
          |  8|  1|  6|
          |  9|  2|  7|
    state-|  0|  3|  8|
          |  1|  4|  9|
          |  2|  5|  0|
          +---+---+---+

If we have a story that says The ticket seller gave a ticket to the customer, then the work-box-office understanding agent lines up the work-box-office planning agent to that state. And other understanding agents such as purchase-ticket line up their corresponding planning agents to their appropriate states.

Thus we have planning agent cylinders and given every input, the understanding agents try to line them up.

Here is an example handled by ThoughtTreasure using this scheme. We input Jim Garnier was sleeping. Information about the character Jim Garnier has already been entered into the knowledge base: where he lives, what the apartment he lives in contains, and so forth. When this is input, the sleep understanding agent spins the sleep planning agent to the SLEEP state.

When we spin a planning agent to a state, we also have to perform some of the subgoals leading up to that state. So as part of the process of spinning the sleep planning agent to the SLEEP state, a subgoal for Jim to be in his bed is activated and achieved, so that Jim is then located in his bed in the grid:

> Jim Garnier was sleeping.
UA_Sleep--PA_Sleep in SLEEP state
           --PA_Ptrans--Jim located in bed:

&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
&                                                               wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwww&
&                                                     wwwwwwwwwww  tttwsss   ht     Aswcccccccccw&
&                                   wwwwwwwwwwwwwwwwwww         w  tttwmss            wcccccccccw&
&wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwccccccccw        w         w   smwmss            wcc       w&
&w  llllllll ss   g Ecccc  ssssssssswccccccccw        w         w   smwmss            wcv       w&
&wa llllllll ss       s    ssssssssswccccccccw        wwww   wwwww   wwmss            wcc       w&
&wA  Jc                           sswccccccccw                        wwwww    wwwwwwwwww    wwww&
&w   cc                           sswccccccccw                                                  w&
&wr                               sswwwwwwwwww                                                  w&
&wr                                 w         **************************************            w&
&wr                                         **wwwwwwwwwwwwwwwwwwwwwwwwwttttttttttttl*          lw&
&w                                         *   ssw                    wttttttttttttt *         tw&
&w                                       **    ssw                    wttttttttttttt  *         w&
&w J                                w  **      ssw                    wA    s          *JJJJJJJJw&
&w                                  w**        Rsw                    w                JJJJJJJJJw&
&w             s                    w           sw                    wls              JJJJJJJJJw&
&w         ttttttttt                wwwwww    wwww                    w s              JJJJJJJJJw&
&wr       sttttttttts      ss     ssw            w                    w s              JJJJJJJJJw&
&wr        ttttttttt       ss     ssw                                 w                JJJJJJJJJw&
&wr            s           ss      Aw                                 w                JJJJJJJJJw&
&w    s                    ss       w                                 w   E            JJJJJJJJJw&
&w  tttt      sssssssssss  ss     rrw                                 w     ttttttJt   JJJJJJJJJw&
&w A          sssssssssss  ss     rrw            w                    w    rrrrr        rrrrr  Aw&
&wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwww    wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwww&
&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&

Then we enter He woke up. The sleep understanding agent then spins the sleep planning agent to the AWAKE state:

> He woke up.
UA_Sleep--PA_Sleep in AWAKE state

Then we input that Jim poured shampoo on his hair. The shower understanding agent spins the shower planning agent to the READY-TO-LATHER state, that is, the state just after shampoo is poured on the hair. In the process of spinning that shower planning agent to the READY-TO-LATHER state, a number of subgoals first must be achieved: Jim must get up out of the bed, walk to the shower, and turn on the shower:

> He poured shampoo on his hair.
UA_Shower--PA_Shower in READY TO LATHER state
            --PA_Ptrans--Jim located in shower:
            --PA_ShowerOn

&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
&                                                               wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwww&
&                                                     wwwwwwwwwww  tttwsss   ht  *  Aswcccccccccw&
&                                   wwwwwwwwwwwwwwwwwww         w  tttwmss      *     wcccccccccw&
&wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwccccccccw        w         w   smwmss      *     wcc       w&
&w  llllllll ss   g Ecccc  ssssssssswccccccccw        w         w   smwmss     *      wcv       w&
&wa llllllll ss       s    ssssssssswccccccccw        wwww   wwwww   wwmss     *      wcc       w&
&wA  Jc                           sswccccccccw                        wwwww   *wwwwwwwwww    wwww&
&w   cc                           sswccccccccw                                 ***              w&
&wr                               sswwwwwwwwww                                    *             w&
&wr                                 w                                              *            w&
&wr                                           wwwwwwwwwwwwwwwwwwwwwwwwwttttttttttttl*          lw&
&w                                             ssw                    wttttttttttttt *         tw&
&w                                             ssw                    wttttttttttttt  *         w&
&w J                                w          ssw                    wA    s          *JJJJJJJJw&
&w                                  w          Rsw                    w                JJJJJJJJJw&
&w             s                    w           sw                    wls              JJJJJJJJJw&
&w         ttttttttt                wwwwww    wwww                    w s              JJJJJJJJJw&
&wr       sttttttttts      ss     ssw            w                    w s              JJJJJJJJJw&
&wr        ttttttttt       ss     ssw                                 w                JJJJJJJJJw&
&wr            s           ss      Aw                                 w                JJJJJJJJJw&
&w    s                    ss       w                                 w   E            JJJJJJJJJw&
&w  tttt      sssssssssss  ss     rrw                                 w     ttttttJt   JJJJJJJJJw&
&w A          sssssssssss  ss     rrw            w                    w    rrrrr        rrrrr  Aw&
&wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwww    wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwww&
&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&

Now we can ask questions of ThoughtTreasure regarding the simulation up to this point:

> Jim lay down on his bed when?
He lay down on his bed on Thursday January 15, 1998 at midnight.
> Jim was asleep when?
He was asleep between Thursday January 15, 1998 at midnight and Thursday
January 15, 1998 at seven am.
> Jim stood up when?
He stood up on Thursday January 15, 1998 at seven am. He stood up on Thursday
January 15, 1998 at midnight.
> Jim was awake when?
He was awake on Thursday January 15, 1998 at seven am. He was awake on Thursday
January 15, 1998 at midnight.

Everything in the simulation is stored using specific timestamps. In fact, everything in the simulation is concrete. Arbitrary decisions are made in the simulation that are subject to later modification. The simulation approach is controversial. In psychology, there is an ongoing debate as to whether people reason using mental models (Johnson-Laird, 1993) or using inference rules (Rips, 1994). In artificial intelligence, there is a ongoing debate as to whether AI programs should use lucid or indirect representations (Davis, 1991). ThoughtTreasure adopts the view that understanding is building models (lucid representations) using multiple representation schemes. Thus a model or simulation consists of a sequence of states, where each state is a snapshot of (1) the mental world of each story character represented using planning agents and emotions, and (2) the physical world represented using grids and device agents.

We can ask more questions:

> Jim was in his foyer when?
He was in his foyer on Thursday January 15, 1998 at midnight.
> Jim was in his bedroom when?
He was in his bedroom between Thursday January 15, 1998 at midnight and
Thursday January 15, 1998 at seven am.
> Jim was in his bathroom when?
He was in his bathroom on Thursday January 15, 1998 at seven am.

In the simulation it turns out Jim was in the foyer before he went to sleep. That is where he was placed by default because the simulation did not know where he was initially. Then ThoughtTreasure assumed he woke up at some normal time, assumed arbitrarily to be 7 a.m.
7. Status and future work

ThoughtTreasure is currently able to handle several stories in several worlds:

    Jim waking up and taking a shower in his apartment,
    Jenny in her grocery store off of a Paris street, and
    two children, Bertie and Lin, in their playroom and bedrooms. 

ThoughtTreasure will continue to be scaled up, by adding more concepts, assertions, scripts, grids, planning agents, and understanding agents.

Manually coding planning and understanding agents is a very time-consuming process because there are so many natural language constructions that correspond to so many different configurations of the simulation. Just for the sleep agents, we have:

    Mary is sleeping.
    Mary is lying awake in her bed.
    Mary was lying asleep in her bed.
    Mary was asleep and Peter did not want to wake her.
    At ten in the morning, Mary was still asleep.
    Mary had only slept a few hours.
    ... 

The use of semi-automated and automated techniques for acquisition will undoubtedly play a larger role in the future of ThoughtTreasure.
References

Alshawi, Hiyan (Ed.). (1992). The Core Language Engine. Cambridge, MA: MIT Press.

Davis, Ernest (1991). Lucid representations (Technical report TR1991-565). Computer Science Department, New York University.

Fellbaum, Christiane (Ed.). (1998). WordNet: An electronic lexical database. Cambridge, MA: MIT Press.

Johnson-Laird, Philip N. (1993). Human and machine thinking. Hillsdale, NJ: Erlbaum.

Landauer, Thomas K. (1986). How much do people remember? Some estimates of the quantity of learned information in long-term memory. Cognitive Science, 10, 477-493.

Lenat, Douglas B. (1995). CYC: A large-scale investment in knowledge infrastructure. Communications of the ACM, 38(11), 33-48.

Minsky, Marvin (1986). The society of mind. New York: Simon and Schuster.

Mueller, Erik T. (1998). Natural language processing with ThoughtTreasure. New York: Signiform.

Mueller, Erik T. (1999). A database and lexicon of scripts for ThoughtTreasure.

Mueller, Erik T. (2000). A calendar with common sense. Proceedings of the 2000 International Conference on Intelligent User Interfaces (pp. 198-201). New York: Association for Computing Machinery.

Narayanan, Srinivas S. (1997). Knowledge-based action representations for metaphor and aspect (KARMA) (Unpublished doctoral dissertation). University of California, Berkeley.

Rips, Lance J. (1994). The psychology of proof. Cambridge, MA: MIT Press.

Schank, Roger C., and Abelson, Robert P. (1977). Scripts, plans, goals, and understanding. Hillsdale, NJ: Lawrence Erlbaum.
Copyright © 2003 Erik T. Mueller. All Rights Reserved.
