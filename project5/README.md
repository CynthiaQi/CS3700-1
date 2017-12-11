Darren Roscoe and Jacob Taylor
CS3700 - Networks and Distributed Systems
Project 5

High Level Approach
----
We started by reading the RAFT paper, then stubbing out the methods we would need using the summary page and creating a Raft class. Then, we followed the project procedure faithfully, starting by implementing the election process and heartbeats before moving on to the state machine and consensus algorithm. Finally, we had to handle the remaining RAFT logic for handling certain edge cases before moving on to debugging the code once we knew the algorithm was broadly correct.

Raft
----

init                        -> Set credentials and initialize data structures

initSocket                  -> Create and open the socket for communication

receive                     -> Event loop that receives messages on the socket and handles them appropriately

process_pre_election_queue  -> Handles messages received during the election once the leader is determined

apply_commits               -> Applies any log entries which are confirmed by the leader

update_commit_index         -> Commits entries with quorum support

update_followers            -> Sends appendEntries to followers where necessary

send_append_entries         -> Send append entries message to a replica

handle_message              -> When we get a message, determine how it should be handled based on type

handle_append_entries       -> Handle appendEntries RPC including heartbeats

handle_append_entries_reply -> Handle appendEntries reply type messages

handle_request_vote         -> Handle requestVote RPC

handle_vote                 -> Handle counting votes in an election

initialize_leader_state     -> Initialize the leader per RAFT specs

handle_get                  -> Respond to GET type messages

handle_put                  -> Respond to PUT type messages

send_heartbeat              -> Send appendEntries heartbeat message

reset_election              -> Clean the slate of the previous election

send_request_vote           -> Send a requestVote RPC

send_fail                   -> Send a failure type message

send_ok                     -> Send an Ok type message

send_redirect               -> Send a redirect type message

printlog                    -> Logs for debugging purposes

Challenges
----

Overall, the most dificult part of the process was not following and implementing the RAFT spec, but instead determining our own mistakes in the interpretation in order to get tests passing. It was incredibly difficult to debug the partition tests, and it was only after much consideration we were able to find the handful of tiny details in the RAFT spec we needed to get right (or had skipped over) to get the tests to pass.

Testing
----

While testing, we generally used a custom printlog method for debugging. Ultimately, it did not help much aside from drawing our attention to what parts of the process were acting strangely. In conjunction with the (rather time-consuming) test suite, we were able to eventually localize our bugs.
