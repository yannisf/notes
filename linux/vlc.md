# VLC Recipies

### Open a stream from the console

    $ cvlc https://stream.radioparadise.com/aac-64

If there are multiple device and audio systems you might have to define the following:
    --aout=alsa --alsa-audio-device="hw:1,0"
### Open a stream from the console and enable the http interface

    $ cvlc -I http --http-port 32555 --http-password secret https://stream.radioparadise.com/aac-64

The above command will open the stream but also enable the http remote control interface on port `32555` with password `secret`
### Open a stream from the console, enable the http interface and stream the output

    $ cvlc -I http --http-port 32555 --http-password secret \ 
        --sout="#standard{access=http,mux=ts,dst=:45000}" \
        https://stream.radioparadise.com/aac-64
    
The above command will stream the input and make it available on http port `45000`. 

To monitor the stream locally use the following command

    $ cvlc -I http --http-port 32555 --http-password secret \ 
        --sout="#duplicate{dst=std{access=http,mux=ts,dst=:45000},dst=display}" \
        https://stream.radioparadise.com/aac-64
    
## Resources

* https://wiki.videolan.org/Documentation:Streaming_HowTo/Advanced_Streaming_Using_the_Command_Line