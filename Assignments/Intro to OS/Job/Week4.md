## John

I ran this on a Debian machine on my personal server to speed things up. I installed using `sudo apt install john`, then ran `sudo john --incremental=digits passwd-old.txt` and had the following output:

```
Loaded 1 password hash (md5crypt [MD5 32/64 X2])
Will run 8 OpenMP threads
Warning: MaxLen = 20 is too large for the current hash type, reduced to 15
Press 'q' or Ctrl-C to abort, almost any other key for status
12345            (pi)
1g 0:00:00:00 20.00g/s 40960p/s 40960c/s 40960C/s 12345..112506
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

So the password is `12345`.

## Jumbo John

```
sudo apt install build-essential libssl-dev
sudo apt install yasm libgmp-dev libpcap-dev libnss3-dev libkrb5-dev pkg-config libbz2-dev zlib1g-dev
sudo apt install git
git clone https://github.com/magnumripper/JohnTheRipper -b bleeding-jumbo john
cd john/src
./configure && make -s clean && make -sj4
cd ..
sudo ./run/john --subsets=abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890 --length=5 ~/passwd-new.txt
```

The process took 21 hours, 39 minutes and 52 seconds on my server (4c/8t, Intel i7 10th mobile) and found the password to be `TCS21`.