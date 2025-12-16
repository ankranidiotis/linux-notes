# Εγκατάσταση Plex Server σε Linux machine

## Προαπαιτούμενα

1. Docker
2. Docker Compose

## Εγκατάσταση

1. Δημιουργούμε τον φάκελο `plex-server` στον φάκελο `home` του χρήστη. 
2. Μέσα στον φάκελο δημιουργούμε το αρχείο `docker-compose.yml` με τον παρακάτω περιεχόμενο:

```yaml
services:
  plex:
    image: lscr.io/linuxserver/plex:latest
    container_name: plex
    network_mode: host
    environment:
      - PUID=1000
      - PGID=1000
      - VERSION=docker
    volumes:
      - ./config:/config
      - /media/nassoskranidiotis/Backup/Movies:/movies  # Θέση στον σκληρό δίσκο όπου αποθηκεύονται οι ταινίες
      - /media/nassoskranidiotis/Backup/Series:/tv    # Θέση στον σκληρό δίσκο όπου αποθηκεύονται οι σειρές
    devices:
      - /dev/dri:/dev/dri
    restart: unless-stopped
```

3. Εκτελούμε τον `docker-compose up -d` για να εκτελέσουμε τον Plex Server.
4. Δημιουργούμε τους φακέλους `/movies` και `/tv` στον σκληρό δίσκο και τοποθετούμε τους ταινίες και τις σειρές. Οι υπότιτλοι να έχουν το ίδιο όνομα με αυτό της ταινίας και κατάληξη `.el.srt`.

## Πρόσβαση στον Plex Server

1. Η πρόσβαση στον Plex Server γίνεται μέσω του `http://localhost:32400`
2. Είναι απαραίτητο να ανοίξουμε την θύρα `32400` στο τείχος προστασίας του λειτουργικού συστήματος.
3. Σύνδεση με προσωπικό λογαριασμό Plex.

## Ρυθμίσεις Plex

1. Settings -> Transcoder -> Show Advanced -> Disable video stream transcoding -> Save
2. Manage -> Libraries -> Add Library και προσθέτουμε το είδος της βιβλιοθήκης (π.χ. Movies) και στην συνέχεια επιλέγουμε τον φάκελο `/movies` (που έχουμε ορίσει μέσα στο Docker container).
3. Στις 3 τελείες δίπλα στο Movies επιλέγουμε Scan Library Files. 

## Επίλυση προβλημάτων

1. Αν το HRD10 ή Dolby Vision δεν παίζει καλά στην τηλεόραση, τότε πρέπει να ενεργοποιηθεί το HDMI Deep Color (LG) σε αυτήν. 
