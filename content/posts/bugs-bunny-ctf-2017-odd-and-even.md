---
title: "Bugs Bunny CTF 2017 Odd And Even"
date: 2017-08-03T21:20:39+10:00
draft: false
tags: [ "Bugs Bunny", "Programming", "Python" ]
---

{{< figure src="/images/bugs-bunny-ctf-2017-odd-and-even.png" title="Odd & Even Challenge" >}}

{{< highlight python >}}
#!/usr/bin/env python

import random
import socket

def find_seed(first, second):
  for i in range(0,2000):
    random.seed(i)
    rand = random.randrange(0,100)
    if rand == first:
      rand = random.randrange(0,100)
      if rand == second:
        return i

def read_chunk():
  data = b''
  while True:
    chunk = client.recv(4096)
    data += chunk
    if len(chunk) < 4096:
      break
  return data

def send_number(i):
  client.send(str(i).encode('utf-8') + b'\n')

seed = find_seed(48, 90)
random.seed(seed)

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(('34.253.165.46', 1995))

# read connect message
data = read_chunk()
send_number(2)
data = read_chunk()

for i in range(0,100):
  rand = random.randrange(0,100)
  if rand % 2 == 0:
    send_number(0)
  else:
    send_number(1)
  data = read_chunk()
  print(data)

client.close()
{{< /highlight >}}