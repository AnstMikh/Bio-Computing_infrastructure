## Theory \[2\]

-   \[0.4\] What are [computer
    ports](https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/)
    on a high level? How many ports are there on a typical computer?

**Computer ports** are software-based unique identifiers helping an OS
to divide information streams. Port is like a door and OS has several
ones. It sends and receives different kinds of information through
different ports. When a package of info is sent from a specific computer
port it has the information of a port through which it should be
recieved in a target computer. And the target computer is \'listening\'
to a certian door to get this kind of information.

Port is an integer number ranging from 0 to 65535, but for TCP zero port
is reserved and cannot be used while in UDP it means the absence of
port. Not all the ports are used in a typical computer. About 18 ports
are intensively used and are commonly known:

**Port number -- Protocol used**

    20 -- File Transfer Protocol (FTP) Data Transfer
    21 -- File Transfer Protocol (FTP) Command Control
    22 -- Secure Shell (SSH) Secure Login
    23 -- Telnet remote login service, unencrypted text messages
    25 -- Simple Mail Transfer Protocol (SMTP) email delivery
    53 -- Domain Name System (DNS) service
    67, 68 -- Dynamic Host Configuration Protocol (DHCP)
    80 -- Hypertext Transfer Protocol (HTTP) used in the World Wide Web
    110 -- Post Office Protocol (POP3)
    119 -- Network News Transfer Protocol (NNTP)
    123 -- Network Time Protocol (NTP)
    143 -- Internet Message Access Protocol (IMAP) Management of digital mail
    161 -- Simple Network Management Protocol (SNMP)
    194 -- Internet Relay Chat (IRC)
    443 -- HTTP Secure (HTTPS) HTTP over TLS/SSL
    546, 547 -- DHCPv6 IPv6 version of DHCP

-   \[0.4\] What is the difference between http, https, ssh, and other
    protocols? In what sense are they similar? Name default ports for
    several data transfer protocols.

All the client-server protocols determine the semantics of client-server
interractions. They work on the application level. When data are
transferred with a protocol it means that protocol-specific blocks of
information are added to the data to be recognizable by a network
participant. The protocol-specific blocks contain some meta-information
about the version of the protocol, date, time, status, etc. The content
of the block is what differs one protocol from another. One more
difference is encryption while protocol is being transfered. HTTP, FTP
have no encryption while HTTPS, SSH or SFTP have.They are used for
different tasks. Roughly speaking, HTML pages are trensfered with
HTTP/HTTPS protocols and SSH, FTP, FTPS, SFTP provide remote work in
file systems.

The most famous are port 80 for http, 443 for https and 20/21 for FTP.

-   \[0.4\] Explain briefly: (1) what is IP, (2) what IPs are called
    \'white\'/public, (3) and what happens when you enter \'google.com\'
    into the web browser.

**IP** is a unique identifier of a device in a global or local computer
network. It works with the help of IP protocol on the hardware-based
network layer. It have two versions: IPv4 looks like this:
`192.0.2.235`; IPv6 looks like this:
`2001:0db8:11a3:09d7:1f34:8a2e:07a0:765d`.

**Public IP** address is an IP address available to Internet users, as
opposed to a **private IP** address, which belongs to a local network.
Several IP numbers are reserved to be used in local networks.

While making a browser request, for example \'HSE\' the URL goes to the
nearest available DNS server. There, the request is resolved to IP
address of the \'HSE\' website and its IP is sent back to my browser.
After that, my browser makes a request using the obtained IP to the
Internet. The HSE web server receives my request and replies with HTML
page.

-   \[0.4\] What is Nginx? How does it work on the high level? List
    several alternative web servers.

**Nginx** is a most widespread web server. It uses to the ports 80 (for
http) and 443 (for https), processes http/https requests and replies to
these requests or proxies them to other servers or other ports of the
same server. It is a software server which must be configured on a
hardware server (or a regular computer) to work correctly.

**Apache** is one of the most famous alternatives of nginx but there are
also LiteSpeed Web Server, Microsoft-IIS, etc.

-   \[0.4\] What is SSH, and for what is it typically used? Explain two
    ways to authenticate in an SSH server in detail. **SSH** is a
    network protocol allowing remote operating system management. It is
    of application level and it uses encryption during information
    transfer.

There are two ways to authenticate in an SSH server.

1.  Authentication by password. In this case, we send a request to an
    SSH server to connect. At this moment, a unique fingerprint is
    created and saved on our machine. It looks like this
    `12:f8:7e:78:61:b4:bf:e2:de:24:15:96:4e:d4:72:53`. The fingerprint
    helps to identify our special client-server connection. It helps to
    prevent the connection of computer with the same IP address to the
    SSH server because such a computer does not have the fingerprint.
    SSH server has a database with clients IPs and their passwords. When
    connection is initiated, SSH server requires to enter a password.
    When password is entered and server checked it the connection
    starts.

2.  Authentication by key couple. **Private and public keys** are
    generated. The private key is stored in a client computer and should
    not be released to the public. Public key is open to everyone and
    makes no sense without the private one. Actually, public key is
    something like a lock. Everyone sees it but only a private key
    holder can open it. To start a connection client sends its public
    key to an SSH server to recognize the client. The fingerprint is
    created as mentioned in \'Authentication by password\'. If the
    server finds match of the public key it sends an information
    encrypted with the help of the public key. It cannot decode the
    information without a private key. It just sends the encrypted
    information to the client who decodes it with its private key.

## Problem \[6.5\]

A real-life situation that occurred to me several times over the years.

Imagine wrapping up a large bioinformatics project and wanting to share
raw data with your colleagues in a friendly and straightforward format.
The best option would be to use an online genome browser and host your
data remotely, so it is easily accessible by anyone with a valid link.
This is exactly what we will be doing here.

*Please consider doing this HW using Linux since setting up the SSH
client on Windows is painful, and I won\'t be able to help you.*

Steps:

-   \[2\] Create a new virtual machine in the Yandex/Mail/etc cloud
    (order at least 10GB of free disk space). Generate SSH key pair and
    use it to connect to your server.
-   \[1\] Download the latest human genome assembly (GRCh38) from the
    Ensemble FTP server
    ([fasta](https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz),
    [GFF3](https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz)).
    Index the fasta using samtools (`samtools faidx`) and GFF3 using
    tabix.
-   \[1\] Select and download BED files for three ChIP-seq and one
    ATAC-seq experiment from the ENCODE (use one tissue/cell line).
    Sort, bgzip, and index them using tabix.
-   \[1\] Download and install [JBrowse 2](https://jbrowse.org/jb2/).
    Create a new jbrowse
    [repository](https://jbrowse.org/jb2/docs/cli/#jbrowse-create-localpath)
    in `/mnt/JBrowse/` (or some other folder).
-   \[0.25\] Install nginx and amend its config(/etc/nginx/nginx.conf)
    to contain the following section:

```{=html}
<!-- -->
```
    http {
      # Don't touch other options!
      # ........
      # ........

      # Add this:
      server {
    		listen 80;
    		index index.html;

    		location /jbrowse/ {
    			alias /mnt/JBrowse/;	
    		}
    	}

      # ........
    }

-   \[0.25\] Restart the nginx (reload its config) and make sure that
    you can access the browser using a link like this:
    `http://64.129.58.13/jbrowse/`. Here `64.129.58.13` is your public
    IP address.
-   \[1\] Add your files to the genome browser and verify that
    everything works as intended. Don\'t forget to
    [index](https://jbrowse.org/jb2/docs/cli/#jbrowse-text-index) the
    genome annotation, so you could later search by gene names.

**Remember to put a persistent link to a session with all your BED files
and the genome annotation in the report. I must be able to access it
without problems.**

### Create a VM

I created VM via Yandex Cloud. I used vCPU=2 and 50%, RAM=4Gb, Ubuntu
20.04 and HDD=15GB

    ssh-keygen -t ed25519

Here is how key looks like

    ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE/aPoG+MwDcc2F3zlY091EhgRxKIyrVkZx9fUqQNzLH anst@LAPTOP-K4CEBDA2

Connect with

    ssh -i hw2 amikhailova@158.160.50.75 

Install everything that we will be needed in SSH server

    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get install -y samtools
    sudo apt-get install -y tabix
    sudo apt-get install -y bedtools

### The latest human genome assembly (GRCh38)

I downloaded fa file and gff3 for hg38 genome. And index then via
samtools and tabix

    wget https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
    gunzip Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
    samtools faidx Homo_sapiens.GRCh38.dna.primary_assembly.fa #genome index

    wget https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz
    gunzip Homo_sapiens.GRCh38.108.gff3.gz
    (grep "^#" Homo_sapiens.GRCh38.108.gff3; grep -v "^#" Homo_sapiens.GRCh38.108.gff3 | sort -t"`printf '\t'`" -k1,1 -k4,4n) | bgzip > sorted.Homo_sapiens.GRCh38.108.gff3.gz;
    tabix -p gff sorted.Homo_sapiens.GRCh38.108.gff3.gz;
    
### Download BED and ATAC-seq files. Sort, bgzip, and index them using
tabix

→ Cell line A549

-   ATAC-seq \-- ENCFF031OEA
    <https://www.encodeproject.org/experiments/ENCSR139OYS/>
-   CHIP-seq (conservative IDR thresholded peaks):
    1.  ENCFF664LSW (TAF1)
        <https://www.encodeproject.org/experiments/ENCSR000BPF/>

    The TFIID basal transcription factor complex plays a major role in
    the initiation of RNA polymerase II (Pol II)-dependent
    transcription. TFIID recognizes and binds promoters with or without
    a TATA box via its subunit TBP, a TATA-box-binding protein, and
    promotes assembly of the pre-initiation complex (PIC). The TFIID
    complex consists of TBP and TBP-associated factors (TAFs), including
    TAF1, TAF2, TAF3, TAF4, TAF5, TAF6, TAF7, TAF8, TAF9, TAF10, TAF11,
    TAF12 and TAF13 (PubMed:33795473). TAF1 is the largest component and
    core scaffold of the TFIID complex, involved in nucleating complex
    assembly (PubMed:25412659, 27007846, 33795473). TAF1 forms a
    promoter DNA binding subcomplex of TFIID
    1.  ENCFF276WTW (REST)
        <https://www.encodeproject.org/experiments/ENCSR000BQP/>

    This gene was initially identified as a transcriptional repressor
    that represses neuronal genes in non-neuronal tissues. However,
    depending on the cellular context, this gene can act as either an
    oncogene or a tumor suppressor. The encoded protein is a member of
    the Kruppel-type zinc finger transcription factor family. It
    represses transcription by binding a DNA sequence element called the
    neuron-restrictive silencer element. The protein is also found in
    undifferentiated neuronal progenitor cells and it is thought that
    this repressor may act as a master negative regulator of
    neurogenesis. Alternatively spliced transcript variants have been
    described.
    1.  ENCFF666WMP (RAD21)
        <https://www.encodeproject.org/experiments/ENCSR000BUC/>

    The protein encoded by this gene is highly similar to the gene
    product of Schizosaccharomyces pombe rad21, a gene involved in the
    repair of DNA double-strand breaks, as well as in chromatid cohesion
    during mitosis. This protein is a nuclear phospho-protein, which
    becomes hyperphosphorylated in cell cycle M phase. The highly
    regulated association of this protein with mitotic chromatin
    specifically at the centromere region suggests its role in sister
    chromatid cohesion in mitotic cells.

All these files are from hw2 for ml.

    wget -nc https://www.encodeproject.org/files/ENCFF664LSW/@@download/ENCFF664LSW.bed.gz  -O chip1.bed.gz
    wget -nc https://www.encodeproject.org/files/ENCFF276WTW/@@download/ENCFF276WTW.bed.gz -O chip2.bed.gz
    wget -nc https://www.encodeproject.org/files/ENCFF666WMP/@@download/ENCFF666WMP.bed.gz -O chip3.bed.gz
    wget -nc https://www.encodeproject.org/files/ENCFF031OEA/@@download/ENCFF031OEA.bed.gz -O atac.bed.gz

    for a in atac chip1 chip2 chip3
    do
        gunzip ${a}.bed.gz
        bedtools sort -i $a.bed > sort_${a}.bed
        bgzip sort_${a}.bed
        tabix sort_${a}.bed.gz
    done

### Install JBrowse 2

I decided to install jbrowse via npm

    sudo apt install npm
    sudo npm install -g @jbrowse/*cli*
    jbrowse create JBrowse

### Install Nginx

    sudo apt-get install -y nginx
    cd /etc/nginx
    sudo nano nginx.conf

I changed conf file by adding server and commenting the line below.

      #include /etc/nginx/sites-enabled/*

      server {
        listen 80 default_server;
        index index.html;
        server_name _;

        # Don't put JBrowse inside the home directory!
        # You will have problems with permissions
        location /jbrowse/ {
          alias /home/amikhailova/JBrowse/;	
        }
      }

Then we should reload nginx and see that our jbrowse is working! Hurray!

    sudo nginx -s reload
    http://158.160.50.75/jbrowse/

### Add (BED & FASTA & GFF3) to genome browser {#add-bed--fasta--gff3-to-genome-browser}

    sudo jbrowse add-assembly Homo_sapiens.GRCh38.dna.primary_assembly.fa --load copy --out /home/amikhailova/JBrowse/

    for i in chip1 chip2 chip3 atac
    do
      gunzip sort_${i}.bed.gz
      awk '{gsub(/^chr/,""); print}' sort_${i}.bed > $(echo sort_${i}.bed| cut -d '.' -f 1)'_renamed.bed'
      bgzip sort_${i}_renamed.bed
      tabix -f sort_${i}_renamed.bed.gz
    done

    sudo jbrowse add-track sorted.Homo_sapiens.GRCh38.108.gff3.gz  --load copy --out /home/amikhailova/JBrowse/

    for i in chip1 chip2 chip3 atac
    do 
      sudo jbrowse add-track sort_${i}_renamed.bed.gz --load copy --out /home/amikhailova/JBrowse/
    done

## Extra points \[1.5\]

-   \[1\] Create a Docker container for running JBrowse 2. It should be
    a self-contained application, listening on the default HTTP port.
    Users must be able to mount directories with custom configs and
    access them later without any problems.

Hint: to specify the config, use the config=PATH query parameter. E.g.
`http://64.129.58.13/jbrowse/?config=my_folder%2Fconfig.json` where
`my_folder%2Fconfig.json` is the
[escaped](https://en.wikipedia.org/wiki/Percent-encoding) path to the
config file.

-   \[0.5\] Give an in-depth explanation of the OSI model and how the
    TCP/IP stack works. Don\'t copy-paste descriptions from the
    internet; paraphrase and shorten as much as possible (imagine
    writing a cheat sheet for yourself).

A logical and conceptual paradigm called **Open Systems Interconnections
(OSI)** describes the networking that open systems use to interact and
communicate with one another.

*Features of the OSI model include:*

Layers should only be formed when specific levels of abstraction are
required; each layer\'s function should be selected in accordance with
internationally accepted standards; and there should be a sufficient
number of layers so that different functions do not overlap with one
another. As each layer in the OSI model depends on the layer below it to
execute basic duties, each layer should be able to offer services to the
layer above it; at the same time, it should be small enough to prevent
the architecture from becoming overly complex. A modification in one
layer shouldn\'t necessitate a change in another.

*Layers of OSI*

1.  *Physical layer*. This layer\'s primary job is to turn the bitstream
    into electrical/optical pulses or radio signals, which can then be
    translated into a physical medium. This layer provides the hardware
    to activate, maintain, and deactivate the physical connections
    between the data link entities in addition to the physical
    connection to the underlying medium. This comprises multiplexing,
    channel identification on the underlying medium, and bit stream
    sequencing. Not to be confused with the medium itself, this.
2.  *Data Link Layer*. The physical layer is driven by the data
    connection layer, which also oversees how it functions. At the
    transmitting end, the data link layer transmits data to the physical
    layer, and at the receiving end, it receives data from the physical
    layer. Additionally, it defines the flow control mechanism between
    the two nodes to prevent buffer overflow on either side of the data
    link connection and makes sure that any faults that may have
    happened during transmission or reception on the physical carrier
    are found and fixed.
3.  *Network Layer*. When sending data to a destination node that is not
    on the same network as the source node, this layer is in charge of
    identifying the appropriate intermediate nodes that may be needed.
    When the underlying data link layer is unable to process a datagram
    that the network layer requests be sent across the network, this
    layer also divides the datagram into smaller pieces.
4.  *Transport layer*. Variable length data sequences can be moved
    functionally and procedurally from source to destination across one
    or more networks thanks to the transport layer. This layer offers
    services without or with connection formation for the session layer
    and is end-to-end. The construction, control, and release of
    connections are under the purview of this layer.
5.  *Session layer*. This layer\'s primary function is to handle the
    data transfer between the presentation layers at the two endpoints
    and to coordinate and synchronize their conversation. The
    connections between apps are established, managed, and terminated by
    this layer. At each endpoint, the session layer establishes,
    manages, and ends exchanges and dialogs between apps.
6.  *Presentation level*. The presentation layer offers independence
    from variations in data format/syntax and offers a consistent
    representation of data exchanged between application objects.
    Sometimes, this layer is referred to as the syntactic layer. In
    order for the application layer to use the data, the presentation
    layer must first transform the data. Application data encryption and
    decryption are also handled by this layer.
7.  *Application Layer*. Since there are no upper layer protocols at the
    application layer, it is the topmost layer in the OSI model.
    Applications of software that must interact with other systems do so
    directly with the OSI application layer. This layer should not be
    confused with application software, which is a program that uses
    software to perform its functions; for instance, Google Chrome is
    software application and HTTP is an application layer protocol.

A transmission control protocol/Internet protocol is **TCP/IP**. This
model aids in figuring out how a certain computer should be linked to
the Internet and how information can be sent between them.

Features of the OSI model include:

Support flexible architecture; Make it simple to add more systems to the
network; Ensure that data arriving out of order must be sequenced; Be a
connection-oriented protocol; Provide reliability; Ensure that data
arriving out of order must be sequenced; Enable flow control so that the
sender never overwhelms the receiver with data.

Four levels.

1.  *Application layer*. The majority of network applications run on
    this layer.
2.  *Transport Layer*. Protocols at the transport layer can guarantee
    the right order of data arrival and address the issue of unreliable
    message delivery (\"did the message reach the recipient?\"). The
    transport protocols in the TCP/IP stack identify the application
    that the data is meant for.
3.  *Network layer*. Data transfer between networks is one of the
    original purposes of the network layer. This layer makes use of
    routers that determine the network address using the network mask in
    order to forward packets to the appropriate network. IP network
    protocol packets may include a code that indicates which next-layer
    protocol should be used to access the packet\'s payload. The
    protocol\'s distinctive IP number is this one.
4.  *Channel Layer*. A method of coding data for transmission as a data
    packet at the physical layer is described by the data link layer.
    For instance, Ethernet carries information about the machine or
    computers for which the packet is intended in the header fields of
    the packet.
