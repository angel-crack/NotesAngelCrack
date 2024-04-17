Will be invoked in the following Conditions, if MRGL contains ANN

| Condition                                                                                           | Audible Announcement                                                                                                            |
| --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| An equal or higher precedence call is in progress.  MLPP only                                       | Equal or higher precedence calls have prevented the completion of your call.  Please hang up and try again.                     |
| A precedence access limitation exists.                                                              | Precedence access limitation has prevented the completion of your call.  Please hang up and try again.                          |
| Someone attempted an unauthorized precedence level.                                                 | The precedence used is not authorized for your line.  Please use and authorized precedence or ask your operator for assistance. |
| The call appears busy or the administrator did not configure the directory number for call waiting. | The number you have dialed is busy and not equipped for call waiting. Please hang up and try again.                             |
| The system cannot complete the call.                                                                | Your call cannot be completed as dialed.  Please consult your directory and call again or ask your operator for assistance.     |
| A service interruption occurred.                                                                    | A service disruption has prevented the completion of your call.  In case of emergency call your operator.                       |

Besides, if we need to signal to the that party

| Announcement | Condition                                                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| RingBack     | Blind transfer of a call established over an H.323 ICT. “Send H225 User info Message” Service Parameter must be set to Use ANN for ringback. |
| RingBack     | Blind transfer of a call established with a SIP client.                                                                                      |
| RingBack     | Blind transfer of a call established over a SIP trunk.                                                                                       |
| Barge Tone   | Before a participant joins an Ad-Hoc conference                                                                                              |
| RingBack     | Blind transfer of any type of endpoint into a conference call                                                                                |

By default for an ANN running on a server that runs CCM service supports 48 simultaneous streams, for lowers bandwidth can be decreased to 24.
