# pdf-tools
The PDF renderer / signer service is based on iText PDF rendering framework. The PDF renderer / singer is a wrapper around this lib. The server is implemented as a web server so the communication is done with GET and POST request.

General function

The PDF renderer / signer service is based on Itext PDF rendering framework. The PDF renderer / singer is a wrapper around this lib. The server is implemented as a web server so the communication is done with HTTP GET and POST request. The server has some monitoring functionalities wich can be reached on this content roots:

    https://localhost:6080/monitoring
    https://localhost:6080/login
    https://localhost:6080/sign 

Installation

    get the latest binary 
    pre requirements JAVA is on the path unzip pdf-server-1.0-bin.tar.gz package to your favourite location
    first start receiptRenderer server with receiptRenderer.cmd ore ./receiptRendererServer.sh
    second start receiptSigner server witch receiptSigner.cmd ore ./receiptSignerServer.sh
    To TEST:
    Sample client with payload call wget command if no wget is installed download it  fromhttp://gnuwin32.sourceforge.net/packages/wget.htm
        wget -T10 --post-file "<YOURINSTALLATIONDIRECTORY>\test\payloadSampleForAddressVerificationLetter.html" -O "<YOURINSTALLATIONDIRECTORY>\AddressLetters.pdf" --header "Accept-Language: e" http://127.0.0.1:5080/

Configuration

Most important part of the configuration
bind address and port for the renderer
Document root will be used as root for all the stuff witch is referenced in the HTML input (like pictures .....)

## The Renderer runs as an server
com.swisssign.pdftools.server.receiptRenderer.bindAddress=127.0.0.1
com.swisssign.pdftools.server.receiptRenderer.bindPort=5080
com.swisssign.pdftools.server.receiptRenderer.documentRoot=https://localhost/

Signer URL and port witch is called from the renderer if singer Enabled is set and the input HTML contains this HTML tag => <td id="signature">xxxxx</td> the pdf will by singed at this postion of the tag.

To get the singer working locali you can add a etc/hosts entry 127.0.0.1 local.im4.signdemo.com this will trick out the host verivier check. And change the singerURL acordently

## Remote (or on the same machine) running Signer. The ssl certificate alternative name needs to match
## with the hostname provided here
com.swisssign.pdftools.server.receiptRenderer.signerEnabled=1
com.swisssign.pdftools.server.receiptRenderer.signerUrl=https://localhost:6080/sign

Importatn add Integer value of the renderer server web certificate be aware not all online converter will work since the serial nummber can get big! (working one; ​http://www.mathsisfun.com/binary-decimal-hexadecimal-converter.html )

## Reference to receiptRenderer configuration. The Renderer comes with a certificate where its serial needs
## to get entered here at allowedSerials
## The Signer runs as an server
com.swisssign.pdftools.server.receiptSigner.allowedSerials=2564229076454579447886772018993221
com.swisssign.pdftools.server.receiptSigner.bindAddress=127.0.0.1
com.swisssign.pdftools.server.receiptSigner.bindPort=6080
com.swisssign.pdftools.server.receiptSigner.threads=20

to kill the running server => netstat -tulpn | grep 5080 => kill XXXXX
to kill the running server => netstat -tulpn | grep 6080 => kill XXXXX

to start rendere: nohup ./receiptRendererServer.sh &
to start singer: nohup ./receiptSignerServer.sh &

General information
The PDF renderer source code can by found here
To build it from source call mvn

mvn clean package

the result is a pdf-server-1.xx-bin.zip file. 
