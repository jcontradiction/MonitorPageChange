#!/bin/bash

# monitor.sh - Monitors a web page for changes
# sends an email notification if the file change

USERNAME="your@gmail.com"
PASSWORD="yourgmailpassword"
URL="https://www.bestbuy.com/site/searchpage.jsp?st=rtx+3080"

for (( ; ; )); do
    mv new.html old.html 2> /dev/null
    curl -A "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.75 Safari/537.36" -L "https://www.bestbuy.com/site/searchpage.jsp?id=pcat17071&st=rtx%203080" | grep 'btn btn-primary btn-sm btn-block btn-leading-ficon add-to-cart-button' > new.html
    DIFF_OUTPUT="$(diff new.html old.html)"
    if [ "0" != "${#DIFF_OUTPUT}" ]; then
        sendEmail -f $USERNAME -s smtp.gmail.com:587 \
            -xu $USERNAME -xp $PASSWORD -t $USERNAME \
            -o tls=auto -u "Web page changed" \
            -m "Visit it at $URL"
        sleep 600
    fi
sleep 60
done
