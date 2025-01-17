# How-To: Claim neurons for seed participants

*A guide for seed contributors to claim their ICP utility tokens as NNS neurons. For controlling the neurons after claiming see this \[<https://medium.com/dfinity/integrating-ledger-nano-with-the-nns-frontend-dapp-user-manual-9c5600925e16> tutorial\] instead.*

## Introduction

You are a *seed participant* if you donated to the DFINITY foundation in February of 2017 to support the development of the Internet Computer. At that time, 30 tokens per Swiss Franc of value donated were allotted to your key. These tokens are now called "ICP utility tokens" or "ICP" for short.

At Genesis Unlock, your ICP were disbursed to you in the form of a basket of 49 voting neurons. These neurons already exist inside the Network Nervous System. Your neurons contain the tokens you have been awarded, which are staked inside.

Your neurons have been configured to vote automatically and are already earning voting rewards for you. You do not need to do anything to initialize your neurons in order to continue earning voting rewards.

The 49 neurons created for you have dissolve delays of 0 days, 30 days, 60 days, 90 days and so on. Apart from the first neuron which has a dissolve delay of 0 days (which can be dissolved immediately), the other dissolve delays may have a small random number of days added to it.

Your neurons have also been pre-aged. At the moment of Genesis Unlock, their age was already set to 18 months old. This is important, because neuron age significantly increases your voting power and the voting rewards you receive.

To control your neurons, you must use the same secret key that you generated when the donation was made. It was recorded by you as a 12-word mnemonic phrase. In order to control your neurons you need to first *claim* your basket of neurons. There are two ways to claim which are explained below in this article:

-   Claiming with Ledger Nano device (coming soon)

-   Claiming with an air-gapped computer + networked computer/smartphone

After claiming, there are two ways to control your neurons:

-   The NNS dapp (GUI) as described in this [tutorial](https://medium.com/dfinity/integrating-ledger-nano-with-the-nns-frontend-dapp-user-manual-9c5600925e16)

-   A command line tool ic-hardware-wallet-cli available on [github](https://github.com/dfinity/nns-dapp/tree/main/ic-hardware-wallet-cli)

## Claiming with a Ledger Nano device (coming soon)

This will come with one of the next updates of the "ICP" app for the Ledger Nano. The current release can manage neurons but cannot yet claim them.

Phase 1 release: The initial Phase 1 release of the "ICP" app for Ledger Nano was targeted at all consumers. It allows people to manage neurons, such as changing the dissolve delay, dissolving, disbursing, managing hotkeys, voting, etc. Of course, it also allows transferring liquid ICP in the native ledger.

Phase 2 release: The following Phase 2 release includes the `claim_neurons` function for seed round neurons. It is an update specifically for seed round folks.

## Claiming with an air-gapped computer

Claiming requires access to your secret mnemonic phrase and to secret keys derived from it. It is highly advised that you use an air-gapped computer for the purpose of claiming. If you are not comfortable with such a setup or with any of the following steps then you have to wait for the next release of the "ICP" app for the Ledger Nano device.

As an air-gapped device you can use a Windows, Linux or MacOS machine. Linux includes Raspberry Pi.

### Download the tools

To download the tools you need a second, networked computer. The tools are called `keysmith` and `quill`. We describe here how to find and download a binary for your architecture. Alternatively, you can compile the tools yourself from the source code available on github for [keysmith](https://github.com/dfinity/keysmith) and [quill](https://github.com/dfinity/quill).

Binaries are available for the following hardware architectures. Here, architecture refers to the air-gapped computer, not the networked computer.

| Hardware                | keysmith      | quill          |
|-------------------------|---------------|----------------|
| Mac, Intel silicon      | darwin-amd64  | macos-x86_64   |
| Mac, Apple silicon (M1) | darwin-arm64  | \-             |
| Linux (x86)             | linux-amd64   | linux-x86_64   |
| Raspberry Pi            | linux-arm32   | arm_32         |
| Linux (arm)             | linux-arm64   | \-             |
| Windows                 | windows-amd64 | windows-x86_64 |

#### Download keysmith

Go to [keysmith](https://github.com/dfinity/keysmith/releases/), choose the latest release (currently v1.6.2) and fetch the `.tar.gz` file that matches the air-gapped machine’s architecture in the table above.

#### Download quill

Go to [quill](https://github.com/dfinity/quill/releases), choose the latest release (must be \>= 0.2.12) and fetch the executable file that matches the air-gapped machine’s architecture in the table above.

### Copy to air-gapped machine

Copy the keysmith `.tar.gz` file and the quill executable from the networked machine to the air-gapped machine. For example, you can do so with a USB drive.

### Verify the hashes

On the air-gapped machine, go to the terminal. Change the directory to the folder where the keysmith .tar.gz files and quill executables are. Compute the SHA256 hashes with the commands

``` bash
openssl dgst -sha256 keysmith-*
```

and

``` bash
openssl dgst -sha256 quill-*
```

The hashes should match the following entries:

``` bash
SHA256(keysmith-darwin-amd64.tar.gz)=
a53bad6fa36c1eb35cd36059ffe9cbf4c063b515e47ccf666b7e1c174a7d1088
SHA256(keysmith-darwin-arm64.tar.gz)=
47932452353fe7f921b4ac41828dd19530ae0c4bdb72bcbb016a0715ca80e879
SHA256(keysmith-linux-amd64.tar.gz)=
cb283dac031d8676f25e72d19115be347d2b85c864a17dd563104bf496b14a06
SHA256(keysmith-linux-arm32.tar.gz)=
b28670e2b3483ea9f9ba691e9f76f99df31b2678db33b69c888fb08b634de162
SHA256(keysmith-linux-arm64.tar.gz)=
ebe9cde3cf440ebbfb53dd10bf7f412cbff8b089551100ee0fa48f3ac9bd66c3
SHA256(keysmith-windows-amd64.tar.gz)=
1ef9b77ccaae980aad4a227fe1a817821245da491a90f0e6ad323426b49ae40a
```

and

``` bash
SHA256(quill-arm_32)=
ebe2506e4dc4422e7670094e8b2b1d854a3b3c317b25c1c88990853d3d85c064
SHA256(quill-linux-x86_64)=
18fc671ee8c96b367875b39470073d68db78d32d242d14d4682025ef2a5d9ad4
SHA256(quill-macos-x86_64)=
97c373ab871be377ac784faff089ca26d23c37725230fb36d78f17d7a73b0867
SHA256(quill-windows-x86_64.exe)=
2542244c9ad3a9baf54bc2227e8c71ea8a8fb9f7e6065cc7a848c7b1cdce906e
```

### Unpack and install

For keysmith:

``` bash
tar -f keysmith-*.tar* -x
sudo install -d /usr/local/bin
sudo install keysmith /usr/local/bin
```

You will be prompted to enter your laptop password. The password itself will not appear, simply type it and press enter.

For quill:

``` bash
mv quill-arm_32 quill
sudo install quill /usr/local/bin
```

## Produce the private key file with keysmith

### Test the installation

On the air-gapped computer run:

``` bash
keysmith
```

You should see:

``` bash
usage: keysmith <command> [<args>]

Available Commands:
    account         Print your account identifier.
    generate        Generate your mnemonic seed and write it to a file.
    legacy-address  Print your legacy address.
    principal       Print your principal identifier.
    private-key     Derive your private key and write it to a file.
    public-key      Print your public key.
    shortlist       Print the available commands.
    version         Print the version number.
    x-private-key   Derive your extended private key and write it to a file.
    x-public-key    Print your extended public key.
```

If you are using macOS, running `keysmith` for the first time might require you to grant permission under System Preferences \> Security & Privacy \> General.

### Enter your mnemonic phrase (aka "seed")

If you confident that your environment is secure, then you are ready to enter your seed for use with `keysmith`. For the duration of your session, you store your seed phrase in an environment variable. It will be eliminated from your system when you turn your computer off.

``` bash
read seed
```

Enter your seed phrase and finish with Return.

If you prefer to not have your seed phrase displayed as you type then use this command instead:

``` bash
read -s seed
```

### Optional: check your legacy address and balance

At this point you can already verify your legacy address and ICPT balance. The legacy address matches to what was formerly called "DFN address" in the Dfinity Chrome extension. You may have copied it from the Chrome extension for your records back when you used the extension.

``` bash
echo $seed | keysmith legacy-address -f -
```

The output is a 40 character hex string. It looks something like this:

``` bash
2d89d96b10f7a9456a9154b2f5309ee70df5bce1
```

You can check your ICPT balance as follows: Go to <https://ic.rocks/principal/renrk-eyaaa-aaaaa-aaada-cai>, look for "Canister interface" and the method "balance". There, paste your DFN address in to the field labeled "text" and click the button labeled "Query". Your ICP balance will appear below "nat32".

### Create your private key (.pem file)

Derive your private key from your seed phrase.

``` bash
echo $seed | keysmith private-key -f -
```

This creates a file `identity.pem` containing your private key.

#### Optional: Store .pem file only in RAM

We will later wipe the identity.pem file from the filesystem. There is however a remaining risk that the data could survive in the disk and later extracted despite it being wiped. It is more secure to create a RAM disk and store the .pem file only in the RAM disk.

##### Create RAM disk on MacOS

Run these commands

``` bash
DISK=$(hdiutil attach -nomount ram://16384)
diskutil erasevolume HFS+ RD $DISK
cd /Volumes/RD
```

before running

``` bash
echo $seed | keysmith private-key -f -
```

##### Create RAM disk on Linux

Run these commands

``` bash
sudo mkdir /mnt/ramdisk
sudo mount -t ramfs keysmith /mnt/ramdisk
sudo mkdir /mnt/ramdisk/workspace
sudo chown $USER /mnt/ramdisk/workspace
cd /mnt/ramdisk/workspace
```

before running

``` bash
echo $seed | keysmith private-key -f -
```

##### Create RAM disk on Windows

Todo

## Submit claim with quill

### Test the installation

On the air-gapped computer run:

``` bash
quill
```

You should see:

``` bash
quill 0.2.12

Ledger & Governance ToolKit for cold wallets

USAGE:
    quill [OPTIONS] <SUBCOMMAND>

OPTIONS:
    -h, --help Print help information
        --pem-file <PEM_FILE> Path to your PEM file (use "-" for STDIN)
    -V, --version Print version information

SUBCOMMANDS:
    account-balance Queries a ledger account balance
    claim-neurons   Claim seed neurons from the Genesis Token Canister
    get-proposal-info
    help            Print this message or the help of the given subcommand(s)
    list-neurons    Signs the query for all neurons belonging to the signin principal
    list-proposals
    neuron-manage   Signs a neuron configuration change
    neuron-stake    Signs topping up of a neuron (new or existing)
    public-ids      Prints the principal id and the account id
    send            Sends a signed message or a set of messages
    transfer        Signs an ICP transfer transaction
```

If you are using macOS, running `quill` for the first time might require you to grant permission under System Preferences \> Security & Privacy \> General.

### Sign the claim request

On the air-gapped computer run:

``` bash
quill --pem-file identity.pem claim-neurons >msg.json
```

### Submit the claim to the IC

#### Option 1: With quill on the networked computer

Copy the resulting file `msg.json` back to the networked computer. On the networked computer, change into the directory where `msg.json` is and run:

``` bash
quill send msg.json
```

Your neurons should now be claimed.

You can double-check if you claim was successful in the following way: Go to <https://ic.rocks/genesis/2d89d96b10f7a9456a9154b2f5309ee70df5bce1> where you replace `2d89d96b10f7a9456a9154b2f5309ee70df5bce1` with your own DFN address. Under "Status" you should see "Claimed".

#### Option 2: Use the QR scanner app

-   Install `qrencode`

-   Run `cat msg.json | gzip -c | base64 | qrencode -o msg.png`

-   Open `msg.png` in an image viewer

-   Open scanner app in a browser on a phone: <https://p5deo-6aaaa-aaaab-aaaxq-cai.raw.ic0.app>

-   Scan QR code and submit

### Clean up the air-gapped computer

If your claim was successful then do not forget to remove the `.pem` file on the air-gapped computer:

``` bash
rm identity.pem
```
