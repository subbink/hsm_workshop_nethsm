------------------------
## Why use an HSM?
Arguments for using an HSM:

-   **Trust & Safety**
    -   DNSSEC and TLS/SSL rely heavily on a web of trust!
    -   Safe key storage, Keys never leave the HSM. Keys are not in memory on your servers.
    -   Safe processing, crypto is done in the HSM.
    -   Think carefully: Can you afford to be the weakest link in a chain?  
        Other people could be dependend on you!
-   **Certification** and transparancy can be factors here.
-   Interesting for **trusted (cloud)computing** (encrypted RAM for VM's in "half-trusted" environment)
-   When moving to the cloud: Possibly a way to **avoid breaking agreements**, maybe even avoid legal risks.
-   **offloading**: the most cpu-intensive crypto processing to the most
    capable hardware.    
    Let your server focus on other stuff. An HSM could maybe handle tens
    of thousands of signatures per second
-   **High Availability and active-active** are very nice features, could have their own
    use cases.  
    E.g. a new key on HA-HSM1 will be propagated to HA-HSM2 as well.  
-   **Easy audits**. No need to explain how you secure your keys, because
    the HSM does all secret keeping for you, and auditors are familiar with them.

---------------------
[Next](https://github.com/niek-sidn/hsm_workshop_nethsm/blob/main/Slide10.md)
