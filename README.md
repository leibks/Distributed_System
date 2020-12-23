* High-Level Approach: Election (leader problem):
We used the election timeout for every server to control the time of
starting a new election. Every server will start from the follower.
When timeout, it will become the candidate and send get vote message to
other replicas for votes. If it gets the majority of votes, it will become
the leader. If not, it will become a follower and start a new election.
The leader will keep sending the append entries (heartbeat) RPC for
remaining its leader status (every server which is not follower or candidate)
will recognize the leader by this leader’s PRC. Continue to send the RPC for
declaring the leader and make all followers remain their roles will help us
pass the unreliable tests. RPC AppendEntry RPC and Heartbeat RPC are periodically sent to followers.
Each RPC contains entries to be committed by followers to their local storage.
A node will start sending RPC to followers once it is elected as the leader.
Upon receiving a rpc message, a node will accept the sender as its leader and become a follower.
crash handling -leader crash If a follower has not received an RPC message from its
leader before a randomized timeout, it will become a candidate and become qualified
to enter the next round of election. follower crash Upon receiving a put request,
the leader will put the message to its log entry and send a customized append entry RPC
to each follower. The alive follower will respond a ‘Put_success’ message to the leader.
If the majority of followers responded, the leader will send an ‘OK’ response to the client to let him know that
this Key value pair has been stored. As a result, the system will function as expected
as long as the majority of followers are alive and can be reached by the leader.

* Partition: When the partition happens and the leader does have the quorum, two rest
of followers will become candidates due to do not getting heartbeats from the leader,
but they cannot become the leader because none of them can receive the majority of votes.
After ending the partition, the leader will make these two candidates become its followers
again. When the partition happens and the leader does not have the quorum,

* Challenges: Solved the unreliable problem, in the beginning, after the one candidate
become the leader, it will send the “announce leader information” to set another serve
to recognize this leader. But when the network is unreliable, some replicas cannot receive
this message, they will not have the leader and start a new election, something wrong for it.
Now, we will make every server to read the heartbeat to recognize the leader and reset its
everything to the follower.

* Tests: We print out information of messages sent between the leader and its follower
to make sure the system works as expected. We also test this application by running test
scripts provided by the professor multiple times. These test scripts imitate network
conditions such as different kinds of a network partition, server crash, and unreliable
network."# Distributed_System"
