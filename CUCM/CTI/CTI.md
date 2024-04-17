Stands for Computer Telephony Integration. It is a [[CUCM]] interface for register, control and monitor devices ⇒ CTI Route Points, CTI Ports and Phones.

It makes possible that applications integrate with CM.

* UCCE
* UCCX
* CER
* AC
* CUE
* Recording Servers

The apps connected through TAPI/JTAPI or QBE Connections.

# Redundancy

## CCM Service Fails

* Based on the CM Group of the DP assigned to the controlled device, CTI will re open with the secondary CM.
* JTAPI/TAPI Notified with "out of service" and "in service"
* If no Calls are active when CCM comes back, the CTI will failback to the primary.

## CTI Service Fails
* Tha App will contact to the secondary CTI Manger Server
* Application will not failback once the service comes back, just with application restart or if the Secondary Fails.