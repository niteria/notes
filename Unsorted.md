#### Bidirectional bash pipe, scripting `nc`.

```
mkpipe pipe1 pipe2; (stdbuf -o0 -i0 ./coin1 < pipe1 | tee pipe2) & \
stdbuf -o0 -i0 nc 0 9007 < pipe2 | tee pipe1
```
