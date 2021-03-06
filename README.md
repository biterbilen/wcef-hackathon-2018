# wcef-hackathon-2018
Guidelines for hackathon at WCEF 2018. 

Note: to qualify for a prize, your submission should be released under a permissive open source license (e.g. Apache, MIT, etc.).

Please join our Slack [group](https://nucypher-kms-slack.herokuapp.com/) and head to the dedicated **wcef-hackathon** channel to get help from NC developers. Also, find us at the hackathon itself to ask questions in person. 

# Background info about NuCypher

NuCypher is a key management system which leverages proxy re-encryption to control access over encrypted data.
You can find the white paper [here](https://cdn2.hubspot.net/hubfs/2807639/NuCypher%20KMS%20Technical%20White%20Paper.pdf?t=1510526466105) (although you don't have to read it all!).

Slides from the workshop are available [here](https://github.com/nucypher/wcef-hackathon-2018/blob/master/slides.pdf).

# Possible tasks for the hackathon

## Implementing proxy re-encryption library in languages other than Python or Javascript

NuCypher uses a split-key proxy re-encryption scheme.

We treat [this module](https://github.com/nucypher/nucypher-pre-python/blob/master/npre/umbral.py) as a reference implementation - for the moment, it's our most readable, self-explaining implementation.

We are also working on a [higher-level implementation that we simply call pyUmbral](https://github.com/nucypher/pyUmbral).  Our flagship product is our KMS, which we will launch in the spring of 2018.  That too is open source, [available here](https://github.com/nucypher/nucypher-kms).

In brief, encryption and decryption is effectively [ECIES](https://en.wikipedia.org/wiki/Integrated_Encryption_Scheme), but the ciphertext (or, more accurately, the ephemeral public key) can be transformed so that the data can be shared with more than one recepient via re-encryption.

Please check out the [tutorial](https://blog.nucypher.com/proxy-re-encryption-playground-in-python-3bc66170b9bf) in order to set up the environment and play with re-encryption.

We need client libraries for the following languages: Go, Rust, C. If you wish to play with low-level cryptography in any of these languages (or even different ones if you like) - feel free to hack on this task.

## UI for mining node management console

Our mining nodes are the critical workhorses of the network.
They stake their tokens and, rather than confirming transactions, they re-encrypt ciphertexts.

Mining works in the following way:

* Miner deposits the token (no less than a specified minimum amount);
* Miner locks the tokens for a specified time T (T >= 1 month);
* Mining occurs. The miner performs the re-encryption task(s) during this period;
* The Miner now has three options – he/she can chose to reinvest block rewards back into mining, take the profits, or to commence a gradual spindown of the node in the time T.

We would love to see visualisations/graphs so that the miner might learn how many tokens he/she could potentially earn with each option over time. 

The UI should be a web UI, and can be built using the web tool of your choice (Python frameworks, nodejs frameworks, etc.) 

## UI for granting permissions

The purpose of NuCypher is key management / access management as a service.
NuCypher enables the granting of permissions to different people (i.e. different public keys + ethereum addresses) either indefinitely, or for a specified timeframe. On-pay conditions can also be added. Permissions also need be revocable at any time.

This can be a web UI or a cross-platform desktop app.
Use your favourite web framework, Qt, Electron, or anything you think is appropriate.


## Public keys/certificates for TLS tied to Ethereum address

Our nodes have to establish TLS connections.
But instead of using DNS, we want to tie the certificates to the Ethereum addresses.
The idea is that the certificate (or its hash) is signed and stored on Ethereum blockchain, and for the sake of TLS it's self-signed.
When the connection is being established, the client will read the Ethereum address info (stored in cert metadata?), and check if this certificate is signed by the owner of that address. If so, the connection is established. If not, it fails.

The code for this task needs to be written in Python.
