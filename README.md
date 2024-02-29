# HPC
HPC - Programmazione Parallela e HPC presso l'Università degli Studi di Parma (6 CFU).

## Setup cluster HPC.unipr.it

### Accesso con password

Connettersi tramite VPN ed eseguire
```bash
ssh nome.cognome@login.hpc.unipr.it
```

### Accesso senza password (Linux)
Assicurarsi di aver installato [OpenSSH](https://en.wikipedia.org/wiki/OpenSSH) sul proprio pc.

1. Come prima cosa bisogna generare la propria coppia di chiavi nel cluster. Dopo aver effettuato l'accesso tramite password, eseguire i comandi:
    ```bash
    ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    chmod 0600 ~/.ssh/authorized_keys
    ```
2. In caso fosse necessario, ripetere lo step precedente nel proprio host (`ls ~/.ssh/` per verificare l'esistenza della coppia di chiavi, ad es. `id_ed25519` e `id_ed25519.pub`).

3. A questo punto bisogna rendere consapevole l'hpc che può dialogare in maniera sicura con il nostro host, aggiungendo la chiave pubblica dell'host al file `~/.ssh/authorized_keys` nella propria home sul cluster. È possibile farlo in automatico con il comando
    ```bash
    ssh-copy-id nome.cognome@login.hpc.unipr.it
    ```
    In alternativa, basta copiare manualmente il contenuto di `~/.ssh/id_ed25519.pub` in fondo al file `~/.ssh/authorized_keys` sul cluster.

### `ssh-agent` (opzionale)

Nel caso in cui si utilizzi una *passphrase* a protezione della propria chiave privata, ad ogni tentativo di accesso ssh questa verrà richiesta; è utile in questo caso utilizzare un `ssh-agent`, un programma che mantiene in memoria la chiave privata decrittata (tramite l'inserimento della passphrase) e la fornisce ai servizi che la richiedono.
Per la configurazione si rimanda a [questa](https://wiki.archlinux.org/title/SSH_keys#SSH_agents) guida.

È conveniente abilitare l'agente subito dopo il login. La suite `openssh` fornisce un `ssh-agent.service` che permette di far partire il servizio come **systemd user unit**:

```bash
systemctl --user enable ssh-agent.service
systemctl --user start ssh-agent.service

# esporta la variabile d'ambiente nella shell bash
cat <<EOF >>~/.bashrc 
# ssh-agent
export SSH_AUTH_SOCK="$XDG_RUNTIME_DIR/ssh-agent.socket"
EOF

# aggiunge la chiave privata
ssh-add ~/.ssh/id_ed25519
```

In questo modo, una volta inserita la passphrase alla prima connessione, l'agente conserverà fino al prossimo riavvio del pc la chiave privata decrittata (salvo differenti specifiche).


# Wiki 
Ecco un semplice elenco di linee guida da seguire per partecipare alla stesura degli appunti e per mantenere il codice il più ordinato e leggibile possibile.
1. Per visualizzare bene gli appunti utilizzeremo _Obsidian_: software che permette di prendere appunti che opera su file Markdown `.md`. La preview che si vede su Github può risultare incompleta o non leggibile.
2. Plugin utilizzati su _Obsidian_:
	- [table-editor-obsidian](https://github.com/tgrosinger/advanced-tables-obsidian)
	- [highlightr-plugin](https://github.com/chetachiezikeuzor/Highlightr-Plugin)
	- [obsidian-automatic-table-of-content](https://github.com/johansatge/obsidian-automatic-table-of-contents)
	- [todo-checklist](https://github.com/delashum/obsidian-checklist-plugin)
	- [admonition](https://github.com/valentine195/obsidian-admonition)
	- [url-into-selection](https://github.com/denolehov/obsidian-url-into-selection)
3. Effettuare un `git fork` della repository in questione, nella quale poi si andranno a modificare / aggiungere appunti
4. Effettuare sempre un `git fetch` e successivamente un `git merge` prima di qualsiasi `git push` e/o `git commit`
5. Non utilizzare caratteri in stampato e caratteri speciali nel nome dei file
6. Utilizzare il carattere `_` al posto degli "spazi" nel nome dei file
7. Utilizzare sempre la numerazione dei file nel nome (i numeri indicano l'ordine degli argomenti trattati a lezione) 
8. Nel file `.md` inserire sempre un indice dinamico (utilizzando il plugin) e un "ritorna all'indice" alla fine di ogni paragrafo / argomento
9. Nel titolo dei `commit` e della  `pull request`, oltre ad inserire il tema trattato, aggiungere anche il proprio username. Ad esempio il titolo di questa `pull request` sarà: `@{tuo_username}: Aggiornamento wiki`

### Esempio 
Il primo argomento trattato a lezione è stato quello del significato di _"Hello, world!"_.  
Il nome file sarà: `01-helloworld.md`.  
Il titolo del commit sarà: `"@{tuo_username}: Aggiunti appunti lezione 01-helloworld"`.  
Il titolo della pull request sarà: `"@{tuo_username}: Appunti lezione 26-02-2023"`.  

## Contributors
<a href="https://github.com/unipr-org/HPC/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=unipr-org/HPC" />
</a>
