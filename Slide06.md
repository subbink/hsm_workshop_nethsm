------------------------------
## What's in it?
At a minimum, an HSM appliance should contain a **crypto module**.  
This is a closed-off 'mini computer' with its own CPU and RAM and
storage inside the appliance/card/stick.  
This module handles all encryption and signing, also stores the keys (or
maybe a key to all keys).  
All "other stuff" like e.g. networking and api-interfacing is handled
by parts outside this crypto module.  
The crypto module in most cases has a very limited interface, you are probably not allowed to log into it.

An HSM should contain an **HRNG** (hardware random number generator),  
because secure key generation is very dependent on a good source of random numbers.

A Linux/Windows server in most cases has no HRNG, but it uses tricks to
create real random.  
E.g. harvesting the Xth decimal of all execution times of processes, or
in the past also timings of key-presses or mouse movement (that was useless on a headless server...).  
A good key generator will use a blocking random device and wait for new
real random bits if this device cannot deliver yet.  
Especially when creating keys: a predictable key may lead to the worst false sense of security.

------------
[Next](https://github.com/niek-sidn/hsm_workshop_nethsm/blob/main/Slide07.md)

## Log into the shell server using the provided credentials
I strongly advise you to use the SIDN provided shell server.  
Just ssh into it with the provided ip adress or hostname and credentials.  
`ssh userX@centrshell.sidn.nl`  
`git clone https://github.com/niek-sidn/hsm_workshop_nethsm.git`  
It has most of the needed packages installed.  
It has all the config files, even if you want to run the exercises directly on your laptop, you will still need a source for copy-pasting.  

## Exercise "Random in bash"
Random: bash random commands use (non-blocking) /dev/urandom, not (almost) real random.\
Run:
```bash
RANDOM=13 && echo $RANDOM
```
You got 21880, didn't you?

---
Now run *twice*:
```bash
echo $RANDOM
```
You got 9438 and 11869 , didn't you?

---
*Now do **both** previous exercises again!*

/dev/urandom generates a predictable, repeatable set of pseudo-random numbers.  
The begin argument is a starting point, and this can of course be a piece of "good" random from the pool.  
But it can also be the output of 'date' at reboot.
> Note: For testing a predictable, repeatable set of pseudo-random numbers can actually be a good thing!

---
Linux does have real random: /dev/random (but it could be blocking)\
Real random:
```bash
cat /proc/sys/kernel/random/poolsize
```
Used to be 4096, but is now 256 (on kernels >5, a switch was made)

---
Available random:
```bash
cat /proc/sys/kernel/random/entropy\_avail
```
On kernels <5 this drops on intensive use of /dev/random (e.g. `od -d /dev/random`)

## Exercise "Random from an HSM"
---
#### Random from an HSM on the net 1  
```bash
cat hsm_env_vars  # provided on the shell server
curl $CURLOPTS --user "$OPERATOR:$OPASS" -X POST $API/random -H 'accept: application/json' -H 'Content-Type: application/json' -d '{ "length": 4 }' | jq -r .random | base64 -d | xxd -p -c 64
```
This works because this specific HSM offers an HTTP interface.

---
#### Random from an HSM on the net 2  
(pkcs11-tool will be explained later)  
```
cat /usr/local/etc/nitrokey/p11nethsm.conf
pkcs11-tool --module /usr/local/lib/nethsm/nethsm-pkcs11-vv1.6.0-x86_64-ubuntu.24.04.so --generate-random 4 | xxd -c 64 -p
# or pkcs11-tool --module $SO_NETHSM --generate-random 4 | xxd -c 64 -p
```
An HSM is pretty much guaranteed to have a PKCS#11 interface

---------------
[Next](https://github.com/niek-sidn/hsm_workshop_nethsm/blob/main/Slide07.md)

