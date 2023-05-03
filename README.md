Download Link: https://assignmentchef.com/product/solved-cs2030-project-discrete-event-simulator
<br>



<table width="689">

 <tbody>

  <tr>

   <td colspan="3" width="689"><strong>CS2030 Project — Discrete Event Simulator</strong>A discrete event simulator is a software that simulates the changes in the state of a system across time, with each transition from one state of the system to another triggered via an event. Such a system can be used to study many complex real-world systems such as queuing to order food at a fast-food restaurant.An event occurs at a particular time, and each event alters the state of the system and may generate more events. States remain unchanged between two events (hence the term <strong>discrete</strong>), and this allows the simulator to jump from the time of one event to another. Some simulation statistics are also typically tracked to measure the performance of the system.The following illustrates a queuing system comprising a single service point <strong>S</strong> with one customer queue.In this exercise, we consider a multi-server system comprising the following:There are a number of <strong>server</strong>s; each server can serve one customer at a time.Each customer has a <strong>service time</strong> (time taken to serve the customer).When a <strong>customer</strong> arrives (<strong>ARRIVE</strong> event): if the server is idle (not serving any customer), then the server starts serving the customer immediately (<strong>SERVE</strong> event). if the server is serving another customer, then the customer that just arrived waits in the queue (<strong>WAIT</strong> event).if the server is serving one customer with a second customer waiting in the queue, and a third customer arrives, then this latter customer leaves (<strong>LEAVE</strong> event). In other words, there is at most one customer waiting in the queue.When the server is done serving a customer (<strong>DONE</strong> event), the server can start serving the customer waiting at the front of the queue (if any). If there is no waiting customer, then the server becomes idle again.Notice from the above description that there are five events in the system, namely: <strong>ARRIVE</strong>, <strong>SERVE</strong>, <strong>WAIT</strong>, <strong>LEAVE</strong> and <strong>DONE</strong>. For each customer, these are the only possible event transitions:ARRIVE → SERVE → DONEARRIVE → WAIT → SERVE → DONEARRIVE → LEAVEIn essence, an event is tied to one customer. Depending on the current state of the system, triggering an event will result in the next state of the system, and possibly the next event. Events are also processed via a queue. At the start of the simulation, the queue only contains the customer arrival events. With every simulation time step, an event is retrieved from the queue to be processed, and any resulting event added to the queue. This process is repeated until there are no events left in the queue.As an example, given the arrival times (represented as a floating point value for simplicity) of three customers and one server, and assuming a constant service time of 1.0,

    <table width="632">

     <tbody>

      <tr>

       <td width="632">0.500 0.6000.700</td>

      </tr>

     </tbody>

    </table>The simulation starts with three arrival events

    <table width="632">

     <tbody>

      <tr>

       <td width="632">&lt;0.500 1 arrives&gt;&lt;0.600 2 arrives&gt;&lt;0.700 3 arrives&gt;</td>

      </tr>

     </tbody>

    </table>The next event to pick is &lt;0.500 1 arrives&gt;. This schedules a SERVE event.

    <table width="632">

     <tbody>

      <tr>

       <td width="632">&lt;0.500 1 serves by 1&gt;&lt;0.600 2 arrives&gt;&lt;0.700 3 arrives&gt;</td>

      </tr>

     </tbody>

    </table>The next event to pick is &lt;0.500 1 served&gt;. This schedules a DONE event.</td>

  </tr>

  <tr>

   <td width="28"> </td>

   <td width="632">&lt;0.600 2 arrives&gt; &lt;0.700 3 arrives&gt;&lt;1.500 1 done serving by 1&gt;</td>

   <td width="28"> </td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td colspan="3" width="689">The next event to pick is &lt;0.600 2 arrives&gt;…This process is repeated until there are no more events.Using our example, the entire simulation run results in the following output, each of the form &lt;time_event_occurred, customer_id, event_details&gt;

    <table width="632">

     <tbody>

      <tr>

       <td width="632">0.500 1 arrives0.500 1 serves by 10.600 2 arrives0.600 2 waits at 10.700 3 arrives0.700 3 leaves1.500 1 done serving by 11.500 2 serves by 12.500 2 done serving by 1</td>

      </tr>

     </tbody>

    </table>Finally, statistics of the system that we need to keep track of are:1. the average waiting time for customers who have been served;2. the number of customers served;3. the number of customers who left without being served.In our example, the end-of-simulation statistics are respectively, [0.450 2 1].<strong>Priority Queuing</strong>The main structure that is used to manage the events in any discrete event simulation is the Priority Queue. Java provides the PriorityQueue (a mutable class) that can be used to keep a collection of elements, where each element is given a certain priority.Elements may be added to the queue with the add(E e) method. Note that the queue is modified;The poll() method may be used to retrieve and remove the element with the highest priority from the queue. It returns an object of type E, or null if the queue is empty. Note that the queue is modified.<strong>Take Note</strong>The requirements of this project will be released in stages. For each level N, you will be given a driver class MainN in the file MainN.java, and you will be required to create the SimulateN class to go along with it.All utility classes are to be packaged in cs2030.util, while the rest of the classes related to the simulation are to be packaged in cs2030.simulator, with SimulateN as the only public facing classes.The <a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Pair.java">Pair</a> and <a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/ImList.java">ImList</a> classes are provided. Compile your code using

    <table width="632">

     <tbody>

      <tr>

       <td width="632">javac -d . *.java</td>

      </tr>

     </tbody>

    </table>and run your program to ensure that everything is still in good working order before proceeding to the next level.<strong>Level 0</strong>Before delving into the implementation of the simulator, you will need to come up with your own generic immutable priority queue, PQ&lt;T&gt;. Specifically, you will need to construct the following:A constructor that takes in a Comparator and creates an empty PQ&lt;T&gt;A method add that takes in an element of type T and returns the resulting PQA method poll that takes no arguments and removes the highest priority element in the queue (as determined by the Comparator). Note that both the element and the resulting queue should be removed, i.e. the return type should be Pair&lt;T, PQ&lt;T&gt;&gt;A method isEmpty() that returns true if the PQ is empty, or false otherwise A toString method that returns a string representation of the PQAs much as possible, you should adopt the <em>immutable delegation pattern</em> for your implementation. Also note that the Pair class has been provided for you.</td>

  </tr>

  <tr>

   <td width="28"> </td>

   <td width="632">jshell&gt; import cs2030.util.* jshell&gt; Comparator&lt;String&gt; cmp = (x, y) -&gt; x.compareTo(y)cmp ==&gt; $Lambda$15/<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="75450d454545454545454d454545144c414145354211164216171411">[email protected]</a> jshell&gt; PQ&lt;String&gt; pq = new PQ&lt;String&gt;(cmp)pq ==&gt; [] jshell&gt; pq.add(“one”).add(“two”).add(“three”)$.. ==&gt; [one, two, three]jshell&gt; pq pq ==&gt; [] jshell&gt; pq.isEmpty()$.. ==&gt; true jshell&gt; pq = pq.add(“one”).add(“two”).add(“three”) pq ==&gt; [one, two, three] jshell&gt; pq.poll()$.. ==&gt; (one, [three, two])jshell&gt; pqpq ==&gt; [one, two, three] jshell&gt; pq.isEmpty()$.. ==&gt; false jshell&gt; pq.poll().first()$.. ==&gt; “one” </td>

   <td width="28"> </td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td rowspan="3" width="28">N<strong>L</strong>YthW dA p cIntiY<strong>L</strong>Ww</td>

   <td width="632">jshell&gt; pq.poll().second().poll().first()$.. ==&gt; “three” jshell&gt; pq = pq.poll().second().poll().second().poll().second() pq ==&gt; [] jshell&gt; pq.isEmpty()$.. ==&gt; true jshell&gt; Comparator&lt;Object&gt; c = (x, y) -&gt; x.hashCode() – y.hashCode()c ==&gt; $Lambda$22/<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="55652d656565656565656d656565376365616515616361373030656c">[email protected]</a> jshell&gt; pq = new PQ&lt;String&gt;(c)pq ==&gt; [] jshell&gt; pq.add(“one”).poll()$.. ==&gt; (one, [])</td>

   <td rowspan="3" width="28">tobet</td>

  </tr>

  <tr>

   <td width="632">ote that the PQ class should be packaged in cs2030.util, together with the Pair class.<strong>evel 1</strong>ou will start by building a <em>walking skeleton</em> of the discrete event simulator. With this framework in place, you would be able to flesh out the details accordinge requirement in subsequent levels. All the classes should reside in the package cs2030.simulator.rite an <em>immutable</em> Customer class to represent each arriving customer that is tagged with a customer ID (of type int), as well as an arrival time (of typeouble).

    <table width="632">

     <tbody>

      <tr>

       <td width="632">jshell&gt; import cs2030.util.* jshell&gt; import cs2030.simulator.* jshell&gt; new Customer(1, 0.5)$.. ==&gt; 1 jshell&gt; new Customer(2, 0.6)$.. ==&gt; 2 jshell&gt; new Customer(3, 0.7)$.. ==&gt; 3</td>

      </tr>

     </tbody>

    </table>n event (i.e. ARRIVE, SERVE, DONE, WAIT, or LEAVE) is associated with a customer and an event time. Events are placed in a priority queue so that they canolled one after another according to the time of occurrence of the event. Let us first create a dummy event EventStub to test that events are processedorrectly using the priority queue, PQ&lt;Event&gt;.the EventStub class, include the constructor that takes in a customer and the event time. Also include an appropriate toString method to return the evenme as a string. ou will also need to define an appropriate EventComparator when creating the priority queue.

    <table width="632">

     <tbody>

      <tr>

       <td width="632">jshell&gt; Event event = new EventStub(new Customer(1, 0.5), 3.7) // event time of 3.7 event ==&gt; 3.700 jshell&gt; Comparator&lt;Event&gt; cmp = new EventComparator()cmp ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="3a7f4c5f544e7955574a5b485b4e55487a08025e080f03020d">[email protected]</a> jshell&gt; PQ&lt;Event&gt; pq = new PQ&lt;Event&gt;(cmp) pq ==&gt; [] jshell&gt; pq = pq.add(new EventStub(new Customer(1, 0.5), 3.5)) pq ==&gt; [3.500] jshell&gt; pq = pq.add(new EventStub(new Customer(2, 0.6), 2.6))pq ==&gt; [2.600, 3.500] jshell&gt; pq = pq.add(new EventStub(new Customer(3, 0.7), 1.7))pq ==&gt; [1.700, 3.500, 2.600] jshell&gt; pq.poll().first()$.. ==&gt; 1.700 jshell&gt; pq.poll().second().poll().first()$.. ==&gt; 2.600 jshell&gt; pq.poll().second().poll().second().poll().first()$.. ==&gt; 3.500 jshell&gt; pq.poll().second().poll().second().poll().second().isEmpty()$.. ==&gt; true</td>

      </tr>

     </tbody>

    </table><strong>evel 2</strong>rite an <em>immutable</em> Server class to represent a server that is tagged with a server ID (of type int). The servers provide their services within a shop. As such, rite a Shop class that takes in a list of servers.</td>

  </tr>

  <tr>

   <td width="632">jshell&gt; import cs2030.util.* jshell&gt; import cs2030.simulator.* jshell&gt; new Server(1)$.. ==&gt; 1 jshell&gt; new Server(2)$.. ==&gt; 2 </td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td rowspan="2" width="28">AnThepair.<u>1</u><u>^D</u>—</td>

   <td width="632">jshell&gt; new Shop(List.&lt;Server&gt;of(new Server(1), new Server(2))) $.. ==&gt; [1, 2]</td>

   <td rowspan="2" width="28">i,</td>

  </tr>

  <tr>

   <td width="632">Now how do we link the customer(s) to the shop (or list of servers)? By using events! Every time an event is polled from the priority queue, the event (e.g. SERVE event) looks at the current state of the shop, and updates the state (e.g. an available server becomes busy) as well as to generate the next event (e.g. generating a DONE event to be added to the priority queue).Event interface is given below; you may modify it to suit your needs later.

    <table width="632">

     <tbody>

      <tr>

       <td width="632">package cs2030.simulator; import java.util.Optional; import cs2030.util.Pair; interface Event {Pair&lt;Optional&lt;Event&gt;, Shop&gt; execute(Shop shop); }</td>

      </tr>

     </tbody>

    </table>execute method takes the current status of the shop, makes changes to the state of the shop based on the event, and returns the next event (if any) andupdated shop status as a pair of values.Now, include the execute method in the EventStub that takes in a Shop, and returns no event (represented by an empty optional) and the same shop as a

    <table width="632">

     <tbody>

      <tr>

       <td width="632">jshell&gt; Shop shop = new Shop(List.&lt;Server&gt;of(new Server(1), new Server(2))) shop ==&gt; [1, 2] jshell&gt; new EventStub(new Customer(1, 0.5), 3.5) $.. ==&gt; 3.500 jshell&gt; new EventStub(new Customer(1, 0.5), 3.5).execute(shop) $.. ==&gt; (Optional.empty, [1, 2])</td>

      </tr>

     </tbody>

    </table>Since events are associated with customers and the shop associated with the servers, a particular instance of the simulation can be completely defined by the priority queue of events, as well as the status of the shop. Write the class Simulate2 with a constructor that takes in the number of servers, together with a List&lt;Double&gt; of customer arrival times, and constructs a priority queue of EventStub events using the arrival time as the event time, and the status of the shop.Include a run method within the Simulator2 class that takes no arguments, such that invoking the method will cause the simulation to start by having the events in the priority queue polled one after another until the queue is empty. The run method should return a string.

    <table width="632">

     <tbody>

      <tr>

       <td width="632">jshell&gt; new Simulate2(1, List.&lt;Double&gt;of(0.5, 0.6, 0.7))$.. ==&gt; Queue: [0.500, 0.600, 0.700]; Shop: [1] jshell&gt; System.out.println(new Simulate2(1, List.&lt;Double&gt;of(0.5, 0.6, 0.7)).run())0.500 0.6000.700— End of Simulation — jshell&gt; new Simulate2(2, List.&lt;Double&gt;of(3.5, 2.6, 1.7))$.. ==&gt; Queue: [1.700, 3.500, 2.600]; Shop: [1, 2] jshell&gt; System.out.println(new Simulate2(1, List.&lt;Double&gt;of(3.5, 2.6, 1.7)).run())1.700 2.6003.500— End of Simulation —</td>

      </tr>

     </tbody>

    </table>Use the program <a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main2.java">Main2.</a><a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main2.java">j</a><a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main2.java">ava</a> provided to test your program as shown below. The first input value is the number of servers. This is followed by a sequence of input arrival times associated with each Customer having an identifier 1, 2, 3, … respectively. User input is <u>underlined</u>. Input is terminated with a ^D (CTRL-d).Take note of the following assumptions:The format of the input is always correct;Output of a double value, say d, is to be formatted with String.format(“%.3f”, d);<strong>Level 3</strong>If you have designed the simulator correctly, this level would only require you to construct the rest of the ARRIVE, SERVE, DONE, WAIT and LEAVE events in order to perform a proper simulation. For each event, you will need to define an appropriate execute method that takes in the current state of the shop, and return the appropriate next event, as well as the updated shop as a pair of values.Using an ARRIVE event at time t as an example, if the execute method takes in a shop with all servers free, then the shop will return a serve event (to be served by 1 at time t), and the shop (no updates are necessary since service has not started). On the other hand, if the event was a SERVE event at time t at then the execute method takes in the shop, and returns a DONE event at time t + 1.0 (assuming service time of 1.0) together with the updated shop with server i tagged as busy.From this point on, you will not be guided with jshell tests. Instead we will only provide sample runs using the provided Main programs.Here’s a tip: rather than creating all five events at the same time, you can still develop and test one event at a time. For example, start with the arrival event, and within it’s execute method, return the EventStub with a later event time. At the same time, you can check the status of the shop to see if the updates are intended. Eventually, all arrival events will be processed and only EventStub events are left in the queue, which are then processed as per the earlier level.Once tested, you can then proceed to let arrive event’s execute return serve event, and have serve event’s execute method return EventStub.</td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td rowspan="3" width="28">ClTWyUcUarM re</td>

   <td width="632">jshell&gt; list.get(0)$.. ==&gt; (0.5, $Lambda$35/<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="75450d454545454545454d45454511454d41453517114d11174014">[email protected]</a>) jshell&gt; list.get(1)$.. ==&gt; (0.6, $Lambda$35/<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0d3d753d3d3d3d3d3d3d353d3d3d693d35393d4d6f6935696f386c">[email protected]</a>) jshell&gt; list.get(2)$.. ==&gt; (0.7, $Lambda$35/<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="f8c880c8c8c8c8c8c8c8c0c8c8c89cc8c0ccc8b89a9cc09c9acd99">[email protected]</a>)</td>

   <td rowspan="3" width="28"> </td>

  </tr>

  <tr>

   <td width="632">early, the three input are associated with the first value of each pair in the list.

    <table width="632">

     <tbody>

      <tr>

       <td width="632">jshell&gt; list.get(0).first()$.. ==&gt; 0.5 jshell&gt; list.get(1).first()$.. ==&gt; 0.6 jshell&gt; list.get(2).first()$.. ==&gt; 0.7</td>

      </tr>

     </tbody>

    </table>he second value of each pair will not read input until the Supplier’s get method is invoked. For example,

    <table width="632">

     <tbody>

      <tr>

       <td width="632">jshell&gt; list.get(1).second()$.. ==&gt; $Lambda$35/<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="45753d757575757575757d75757521757d71750527217d21277024">[email protected]</a> jshell&gt; list.get(1).second().get()1.0$.. ==&gt; 1.0</td>

      </tr>

     </tbody>

    </table>hat is interesting in the above is that the second customer with the arrival time of 0.6, is the first one to be served with a new input of 1.0. Also note that ifou invoke the same jshell test again, another input is read and returned, rather than returning the value that was last read.

    <table width="632">

     <tbody>

      <tr>

       <td width="632">jshell&gt; list.get(1).second().get() // same test2.0$.. ==&gt; 2.0</td>

      </tr>

     </tbody>

    </table>se the program <a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main5.java">Main5.</a><a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main5.java">j</a><a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main5.java">ava</a> provided to test your program. The Lazy class in <a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Lazy.java">Lazy.</a><a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Lazy.java">j</a><a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Lazy.java">ava</a> is also provided; use it if you need to cache the service time of theustomer.ser input starts with an integer value representing the number of servers, and an integer value representing the number of customers. This is followed by the rival times of the customers. Lastly, a number of service times (could be more than necessary) are provided.oreover, rather than typing out the user input, it would be more convenient if you save the input in a file, say test5_1.in and pipe the contents of the file ordirect the file as part of user input.To pipe the contents of the file, use: cat test5_1.in | java Main5To redirect the file, use: java Main5 &lt; test5_1.in

    <table width="632">

     <tbody>

      <tr>

       <td width="632">$ cat test5_1.in1 30.500 0.6000.7001.0 1.01.0$ cat test5_1.in | java Main50.500 1 arrives0.500 1 serves by 10.600 2 arrives0.600 2 waits at 10.700 3 arrives0.700 3 leaves1.500 1 done serving by 11.500 2 serves by 12.500 2 done serving by 1[0.450 2 1]</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

  <tr>

   <td width="632">$ cat test5_2.in1 60.500  0.600  0.700  1.500  1.6001.7001.01.01.0$ cat test5_2.in | java Main50.500 1 arrives0.500 1 serves by 10.600 2 arrives0.600 2 waits at 10.700 3 arrives0.700 3 leaves1.500 1 done serving by 11.500 2 serves by 11.500 4 arrives1.500 4 waits at 11.600 5 arrives1.600 5 leaves1.700 6 arrives1.700 6 leaves2.500 2 done serving by 1</td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td rowspan="3" width="28"> </td>

   <td width="632">2.500 4 serves by 13.500 4 done serving by 1[0.633 3 3]</td>

   <td rowspan="3" width="28"> </td>

  </tr>

  <tr>

   <td width="632">


    <table width="632">

     <tbody>

      <tr>

       <td width="632">$ cat test5_3.in2 100.000000 0.313508 1.204910 2.776499 3.876961 3.909737 9.006391 9.043361 9.105379 9.159630 0.313141 0.103753 0.699522 0.014224 0.154173 0.012659 1.477717 2.593020 0.2968470.051732$ cat test5_3.in | java Main50.000 1 arrives0.000 1 serves by 10.313 1 done serving by 10.314 2 arrives0.314 2 serves by 10.417 2 done serving by 11.205 3 arrives1.205 3 serves by 11.904 3 done serving by 12.776 4 arrives2.776 4 serves by 12.791 4 done serving by 13.877 5 arrives3.877 5 serves by 13.910 6 arrives3.910 6 serves by 23.922 6 done serving by 24.031 5 done serving by 19.006 7 arrives9.006 7 serves by 19.043 8 arrives9.043 8 serves by 29.105 9 arrives9.105 9 waits at 19.160 10 arrives9.160 10 waits at 210.484 7 done serving by 110.484 9 serves by 110.781 9 done serving by 111.636 8 done serving by 211.636 10 serves by 211.688 10 done serving by 2[0.386 10 0]</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

  <tr>

   <td width="632">$ cat test5_4.in2 100.000000 0.313141 0.416894 1.116416 1.130640 1.284813 1.297472 2.775189 5.368209 5.665056 0.313263 2.645188 2.701273 0.263761 1.481332 0.4150310.214836$ cat test5_4.in | java Main50.000 1 arrives0.000 1 serves by 10.313 2 arrives0.313 2 serves by 20.313 1 done serving by 10.417 3 arrives0.417 3 serves by 11.116 4 arrives1.116 4 waits at 11.131 5 arrives1.131 5 waits at 21.285 6 arrives1.285 6 leaves1.297 7 arrives</td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td rowspan="3" width="28"><strong>L</strong>EWstUU fo</td>

   <td width="632">1.297 7 leaves2.775 8 arrives2.775 8 leaves2.958 2 done serving by 22.958 5 serves by 23.118 3 done serving by 13.118 4 serves by 13.222 5 done serving by 24.599 4 done serving by 15.368 9 arrives5.368 9 serves by 15.665 10 arrives5.665 10 serves by 25.783 9 done serving by 15.880 10 done serving by 2[0.547 7 3]</td>

   <td rowspan="3" width="28"> </td>

  </tr>

  <tr>

   <td width="632"><strong>evel 6</strong>ach server now has a queue of customers to allow multiple customers to queue up. A customer that chooses the first queue available and joins at the tail. hen a server is done serving a customer, it serves the next waiting customer at the head of the queue. Hence, the queue should be a first-in-first-out (FIFO)ructure.se the program <a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main6.java">Main6.</a><a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main6.java">j</a><a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main6.java">ava</a> provided to test your program.ser input starts with three integer values representing the number of servers, followed by the maximum queue length, and the number of customers. This is llowed by the arrival times of the customers. Lastly, a number of service times (could be more than necessary) are provided.

    <table width="632">

     <tbody>

      <tr>

       <td width="632">$ cat test6_1.in2 1 100.000000 0.313508 1.204910 2.776499 3.876961 3.909737 9.006391 9.043361 9.105379 9.159630 0.313141 0.103753 0.699522 0.014224 0.154173 0.012659 1.477717 2.593020 0.2968470.051732$ cat test6_1.in | java Main60.000 1 arrives0.000 1 serves by 10.313 1 done serving by 10.314 2 arrives0.314 2 serves by 10.417 2 done serving by 11.205 3 arrives1.205 3 serves by 11.904 3 done serving by 12.776 4 arrives2.776 4 serves by 12.791 4 done serving by 13.877 5 arrives3.877 5 serves by 13.910 6 arrives3.910 6 serves by 23.922 6 done serving by 24.031 5 done serving by 19.006 7 arrives9.006 7 serves by 19.043 8 arrives9.043 8 serves by 29.105 9 arrives9.105 9 waits at 19.160 10 arrives9.160 10 waits at 210.484 7 done serving by 110.484 9 serves by 110.781 9 done serving by 111.636 8 done serving by 211.636 10 serves by 211.688 10 done serving by 2[0.386 10 0]</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

  <tr>

   <td width="632">$ cat test6_2.in2 2 100.000000 0.313508 1.204910 2.776499 3.876961 3.909737 9.0063919.043361</td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td rowspan="3" width="28"> </td>

   <td width="632">9.105379 9.159630 0.313141 0.103753 0.699522 0.014224 0.154173 0.012659 1.477717 2.593020 0.2968470.051732$ cat test6_2.in | java Main60.000 1 arrives0.000 1 serves by 10.313 1 done serving by 10.314 2 arrives0.314 2 serves by 10.417 2 done serving by 11.205 3 arrives1.205 3 serves by 11.904 3 done serving by 12.776 4 arrives2.776 4 serves by 12.791 4 done serving by 13.877 5 arrives3.877 5 serves by 13.910 6 arrives3.910 6 serves by 23.922 6 done serving by 24.031 5 done serving by 19.006 7 arrives9.006 7 serves by 19.043 8 arrives9.043 8 serves by 29.105 9 arrives9.105 9 waits at 19.160 10 arrives9.160 10 waits at 110.484 7 done serving by 110.484 9 serves by 110.781 9 done serving by 110.781 10 serves by 110.833 10 done serving by 111.636 8 done serving by 2[0.300 10 0]</td>

   <td rowspan="3" width="28"> </td>

  </tr>

  <tr>

   <td width="632"> </td>

  </tr>

  <tr>

   <td width="632">$ cat test6_3.in2 2 100.000000 0.313141 0.416894 1.116416 1.130640 1.284813 1.297472 2.775189 5.368209 5.665056 0.313263 2.645188 2.701273 0.263761 1.481332 0.415031 0.214836 3.5126540.209277$ cat test6_3.in | java Main60.000 1 arrives0.000 1 serves by 10.313 2 arrives0.313 2 serves by 20.313 1 done serving by 10.417 3 arrives0.417 3 serves by 11.116 4 arrives1.116 4 waits at 11.131 5 arrives1.131 5 waits at 11.285 6 arrives1.285 6 waits at 21.297 7 arrives1.297 7 waits at 22.775 8 arrives2.775 8 leaves2.958 2 done serving by 22.958 6 serves by 23.118 3 done serving by 13.118 4 serves by 13.222 6 done serving by 23.222 7 serves by 23.637 7 done serving by 24.599 4 done serving by 14.599 5 serves by 14.814 5 done serving by 1</td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td rowspan="3" width="28"><strong>L</strong>T athJsiaUsT c</td>

   <td width="632">5.368 9 arrives5.368 9 serves by 15.665 10 arrives5.665 10 serves by 25.874 10 done serving by 28.881 9 done serving by 1[1.008 9 1]</td>

   <td rowspan="3" width="28"> </td>

  </tr>

  <tr>

   <td width="632"><strong>evel 7</strong>he servers are now allowed to take occasional breaks. When a server finishes serving a customer, there is a chance that the server takes a rest for a certainmount of time. During the break, the server does not serve the next waiting customer. Upon returning from the break, the server serves the next customer ine queue (if any) immediately.ust like customer service time, we cannot pre-determine what rest time is accorded to which server at the beginning; it can only be decided during the mulation. That is to say, whenever a server rests, it will then know how much time it has to rest. The service time is supplied by a random number generators shown in the program <a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main7.java">Main7.</a><a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main7.java">j</a><a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main7.java">ava</a> provided.ser input starts with values representing the number of servers, followed by the maximum queue length, the number of customers and the probability of aerver resting. This is followed by the arrival times of the customers. Lastly, a number of service times (could be more than necessary) are provided.wo sample runs are shown below: the first with no rest, and the second with probability of rest set to 0.5. In this latter case, after server 1 is done serving ustomer 2 at 0.417, he/she takes a rest. In the meantime, customers 3 and 4 are served by server 2. Server 1 resumes serving customer 5 after resting.

    <table width="632">

     <tbody>

      <tr>

       <td width="632">$ cat 7_1.in2 2 10 00.000000 0.313508 1.204910 2.776499 3.876961 3.909737 9.006391 9.043361 9.105379 9.159630 0.313141 0.103753 0.699522 0.014224 0.154173 0.012659 1.477717 2.593020 0.2968470.051732$ cat 7_1.in | java Main70.000 1 arrives0.000 1 serves by 10.313 1 done serving by 10.314 2 arrives0.314 2 serves by 10.417 2 done serving by 11.205 3 arrives1.205 3 serves by 11.904 3 done serving by 12.776 4 arrives2.776 4 serves by 12.791 4 done serving by 13.877 5 arrives3.877 5 serves by 13.910 6 arrives3.910 6 serves by 23.922 6 done serving by 24.031 5 done serving by 19.006 7 arrives9.006 7 serves by 19.043 8 arrives9.043 8 serves by 29.105 9 arrives9.105 9 waits at 19.160 10 arrives9.160 10 waits at 110.484 7 done serving by 110.484 9 serves by 110.781 9 done serving by 110.781 10 serves by 110.833 10 done serving by 111.636 8 done serving by 2[0.300 10 0]</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

  <tr>

   <td width="632">$ cat 7_2.in2 2 10 0.50.000000 0.313508 1.204910 2.776499 3.876961 3.909737 9.006391 9.043361 9.105379 9.159630 0.3131410.103753</td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td rowspan="3" width="28"><strong>L</strong>T          oTUU          th pr</td>

   <td width="632">0.699522 0.014224 0.154173 0.012659 1.477717 2.593020 0.2968470.051732$ cat 7_2.in | java Main70.000 1 arrives0.000 1 serves by 10.313 1 done serving by 10.314 2 arrives0.314 2 serves by 10.417 2 done serving by 11.205 3 arrives1.205 3 serves by 21.904 3 done serving by 22.776 4 arrives2.776 4 serves by 22.791 4 done serving by 23.877 5 arrives3.877 5 serves by 13.910 6 arrives3.910 6 serves by 23.922 6 done serving by 24.031 5 done serving by 19.006 7 arrives9.006 7 serves by 19.043 8 arrives9.043 8 serves by 29.105 9 arrives9.105 9 waits at 19.160 10 arrives9.160 10 waits at 110.484 7 done serving by 110.484 9 serves by 110.781 9 done serving by 111.636 8 done serving by 214.644 10 serves by 114.696 10 done serving by 1[0.686 10 0]</td>

   <td rowspan="3" width="28">nd</td>

  </tr>

  <tr>

   <td width="632"><strong>evel 8</strong>here are now N<sub>self</sub> self-checkout counters set up. In particular, if there are k human servers, then the self-checkout counters are identified from k + 1nwards.ake note of the following:All self-checkout counters share the same queue.Unlike human servers, self-checkout counters do not rest.When we print out the wait event, we always say that the customer is waiting for the self-checkout counter k + 1, even though this customer may eventually be served by another self-checkout counter.se the program <a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main8.java">Main8.</a><a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main8.java">j</a><a href="https://codecrunch.comp.nus.edu.sg/taskfile.php/5229/Main8.java">ava</a> provided to test your program.ser input starts with values representing the number of servers, the number of self-check counters, the maximum queue length, the number of customers a e probability of a server resting. This is followed by the arrival times of the customers. Lastly, a number of service times (could be more than necessary) are ovided.</td>

  </tr>

  <tr>

   <td width="632">$ cat 8_1.in2 0 2 20 0.50.000000 0.313508 1.204910 2.776499 3.876961 3.909737 9.006391 9.043361 9.105379 9.1596309.22461410.147994 11.204933 12.428914 13.109178 15.263626 15.524296 15.939974 17.79309618.7654230.313141 0.103753 0.699522 0.014224 0.154173 0.012659 1.477717 2.593020 0.296847 0.051732 3.4895950.368667</td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td rowspan="3" width="28"> </td>

   <td width="632">0.160532 2.869150 0.895790 1.043020 0.006333 0.5763510.742370$ cat 8_1.in | java Main80.000 1 arrives0.000 1 serves by 10.313 1 done serving by 10.314 2 arrives0.314 2 serves by 10.417 2 done serving by 11.205 3 arrives1.205 3 serves by 21.904 3 done serving by 22.776 4 arrives2.776 4 serves by 22.791 4 done serving by 23.877 5 arrives3.877 5 serves by 13.910 6 arrives3.910 6 serves by 23.922 6 done serving by 24.031 5 done serving by 19.006 7 arrives9.006 7 serves by 19.043 8 arrives9.043 8 serves by 29.105 9 arrives9.105 9 waits at 19.160 10 arrives9.160 10 waits at 19.225 11 arrives9.225 11 waits at 210.148 12 arrives10.148 12 waits at 210.484 7 done serving by 110.484 9 serves by 110.781 9 done serving by 111.205 13 arrives11.205 13 waits at 111.636 8 done serving by 211.636 11 serves by 211.688 11 done serving by 211.688 12 serves by 212.429 14 arrives12.429 14 waits at 213.109 15 arrives13.109 15 waits at 214.644 10 serves by 115.013 10 done serving by 115.178 12 done serving by 215.178 14 serves by 215.264 16 arrives15.264 16 waits at 115.338 14 done serving by 215.338 15 serves by 215.524 17 arrives15.524 17 waits at 215.940 18 arrives15.940 18 waits at 217.793 19 arrives17.793 19 leaves18.207 15 done serving by 218.207 17 serves by 218.765 20 arrives18.765 20 waits at 219.103 17 done serving by 219.103 18 serves by 220.146 18 done serving by 240.474 13 serves by 140.480 13 done serving by 140.480 16 serves by 141.056 16 done serving by 157.110 20 serves by 257.852 20 done serving by 2[6.025 19 1]</td>

   <td rowspan="3" width="28"> </td>

  </tr>

  <tr>

   <td width="632"> </td>

  </tr>

  <tr>

   <td width="632">$ cat 8_2.in2 1 2 20 0.50.000000 0.313508 1.204910 2.776499 3.876961 3.909737 9.006391 9.043361 9.105379 9.1596309.22461410.14799411.204933</td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td rowspan="3" width="28"> </td>

   <td width="632">12.428914 13.109178 15.263626 15.524296 15.939974 17.79309618.7654230.313141 0.103753 0.699522 0.014224 0.154173 0.012659 1.477717 2.593020 0.296847 0.051732 3.489595 0.368667 0.160532 2.869150 0.895790 1.043020 0.006333 0.576351 0.7423703.007573$ cat 8_2.in | java Main80.000 1 arrives0.000 1 serves by 10.313 1 done serving by 10.314 2 arrives0.314 2 serves by 10.417 2 done serving by 11.205 3 arrives1.205 3 serves by 21.904 3 done serving by 22.776 4 arrives2.776 4 serves by 22.791 4 done serving by 23.877 5 arrives3.877 5 serves by 13.910 6 arrives3.910 6 serves by 23.922 6 done serving by 24.031 5 done serving by 19.006 7 arrives9.006 7 serves by 19.043 8 arrives9.043 8 serves by 29.105 9 arrives9.105 9 serves by self-check 39.160 10 arrives9.160 10 waits at 19.225 11 arrives9.225 11 waits at 19.402 9 done serving by self-check 310.148 12 arrives10.148 12 serves by self-check 310.200 12 done serving by self-check 310.484 7 done serving by 110.484 10 serves by 111.205 13 arrives11.205 13 serves by self-check 311.574 13 done serving by self-check 311.636 8 done serving by 212.429 14 arrives12.429 14 serves by self-check 312.589 14 done serving by self-check 313.109 15 arrives13.109 15 serves by self-check 313.974 10 done serving by 113.974 11 serves by 114.869 11 done serving by 115.264 16 arrives15.264 16 serves by 115.524 17 arrives15.524 17 serves by 215.531 17 done serving by 215.940 18 arrives15.940 18 waits at 115.978 15 done serving by self-check 316.307 16 done serving by 116.307 18 serves by 116.883 18 done serving by 117.793 19 arrives17.793 19 serves by 118.535 19 done serving by 118.765 20 arrives18.765 20 serves by 121.773 20 done serving by 1[0.322 20 0]</td>

   <td rowspan="3" width="28"> </td>

  </tr>

  <tr>

   <td width="632"> </td>

  </tr>

  <tr>

   <td width="632">$ cat 8_3.in1 2 2 20 0.5</td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td width="28"> </td>

   <td width="632">0.000000 0.313508 1.204910 2.776499 3.876961 3.909737 9.006391 9.043361 9.105379 9.1596309.22461410.147994 11.204933 12.428914 13.109178 15.263626 15.524296 15.939974 17.79309618.7654230.313141 0.103753 0.699522 0.014224 0.154173 0.012659 1.477717 2.593020 0.296847 0.051732 3.489595 0.368667 0.160532 2.869150 0.895790 1.043020 0.006333 0.576351 0.7423703.007573$ cat 8_3.in | java Main80.000 1 arrives0.000 1 serves by 10.313 1 done serving by 10.314 2 arrives0.314 2 serves by 10.417 2 done serving by 11.205 3 arrives1.205 3 serves by self-check 21.904 3 done serving by self-check 22.776 4 arrives2.776 4 serves by self-check 22.791 4 done serving by self-check 23.877 5 arrives3.877 5 serves by 13.910 6 arrives3.910 6 serves by self-check 23.922 6 done serving by self-check 24.031 5 done serving by 19.006 7 arrives9.006 7 serves by 19.043 8 arrives9.043 8 serves by self-check 29.105 9 arrives9.105 9 serves by self-check 39.160 10 arrives9.160 10 waits at 19.225 11 arrives9.225 11 waits at 19.402 9 done serving by self-check 310.148 12 arrives10.148 12 serves by self-check 310.200 12 done serving by self-check 310.484 7 done serving by 110.484 10 serves by 111.205 13 arrives11.205 13 serves by self-check 311.574 13 done serving by self-check 311.636 8 done serving by self-check 212.429 14 arrives12.429 14 serves by self-check 212.589 14 done serving by self-check 213.109 15 arrives13.109 15 serves by self-check 213.974 10 done serving by 114.823 11 serves by 115.264 16 arrives15.264 16 serves by self-check 315.524 17 arrives15.524 17 waits at 115.718 11 done serving by 115.718 17 serves by 115.725 17 done serving by 115.940 18 arrives15.940 18 serves by 115.978 15 done serving by self-check 2</td>

   <td width="28"> </td>

  </tr>

 </tbody>

</table>




<table width="689">

 <tbody>

  <tr>

   <td rowspan="3" width="28"> </td>

   <td width="632">16.307 16 done serving by self-check 316.516 18 done serving by 117.793 19 arrives17.793 19 serves by self-check 218.535 19 done serving by self-check 218.765 20 arrives18.765 20 serves by self-check 221.773 20 done serving by self-check 2 [0.356 20 0]</td>

   <td rowspan="3" width="28"> </td>

  </tr>

  <tr>

   <td width="632"> </td>

  </tr>

  <tr>

   <td width="632">$ cat 8_4.in1 2 2 20 0.50.000000 0.313508 1.204910 2.776499 3.876961 3.909737 9.006391 9.043361 9.105379 9.1596309.22461410.147994 11.204933 12.428914 13.109178 15.263626 15.524296 15.939974 17.79309618.7654233.131408 1.037533 6.995223 0.142237 1.5417260.12659014.77717225.9301992.9684700.51732134.8959503.6866751.60531628.6915008.957896 1.043020 0.006333 0.576351 0.7423703.007573$ cat 8_4.in | java Main80.000 1 arrives0.000 1 serves by 10.314 2 arrives0.314 2 serves by self-check 21.205 3 arrives1.205 3 serves by self-check 31.351 2 done serving by self-check 22.776 4 arrives2.776 4 serves by self-check 22.919 4 done serving by self-check 23.131 1 done serving by 13.877 5 arrives3.877 5 serves by 13.910 6 arrives3.910 6 serves by self-check 24.036 6 done serving by self-check 25.419 5 done serving by 18.200 3 done serving by self-check 39.006 7 arrives9.006 7 serves by 19.043 8 arrives9.043 8 serves by self-check 29.105 9 arrives9.105 9 serves by self-check 39.160 10 arrives9.160 10 waits at 19.225 11 arrives9.225 11 waits at 110.148 12 arrives10.148 12 waits at self-check 211.205 13 arrives11.205 13 waits at self-check 212.074 9 done serving by self-check 312.074 12 serves by self-check 312.429 14 arrives12.429 14 waits at self-check 212.591 12 done serving by self-check 312.591 13 serves by self-check 313.109 15 arrives13.109 15 waits at self-check 215.264 16 arrives15.264 16 leaves</td>

  </tr>

  <tr>

   <td rowspan="2" width="28"> </td>

   <td width="632">15.524 17 arrives15.524 17 leaves15.940 18 arrives15.940 18 leaves17.793 19 arrives17.793 19 leaves18.765 20 arrives18.765 20 leaves23.784 7 done serving by 124.631 10 serves by 128.318 10 done serving by 128.318 11 serves by 129.923 11 done serving by 134.974 8 done serving by self-check 234.974 14 serves by self-check 247.487 13 done serving by self-check 347.487 15 serves by self-check 356.445 15 done serving by self-check 363.665 14 done serving by self-check 2 [6.320 15 5]</td>

   <td rowspan="2" width="28"> </td>

  </tr>

  <tr>

   <td width="632"><strong><em>Remaining level(s) for correctness/style checking…</em></strong></td>

  </tr>

 </tbody>

</table>

<strong>Submission (Course)</strong>