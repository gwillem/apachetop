    Parse Apache extended status (http://localhost/server-status) and 
    print results, suitable for commandline scripts.
    
    Examples:
    
    Show all process that have been busy for at least 2 minutes with the 
    current request and are in R(eading) state
    
        apachetop --mintime 120 --state R
    
    Show all active Apache processes and reverse sort based on request 
    duration.
     
        apachetop | sort -nk4
    
    Kill all processes that are running for longer than 2 minutes:
    
        apachetop --mintime 120 --kill
    
    Find longest running apache procs on all web and app servers and 
    sort on duration
    
        pboxes -f web,app  --ssh "apachetop --mintime 600" | sort -nk7
    
    Find all "graciously closing" procs, that will probably never ever 
    close. Kill them
    
        apachetop --state G --mintime 120 --kill
    
    Kill all connections occupied by Chinese with crappy bandwidth.
    
        apachetop --state R  --mintime 10 --kill
    
    Kill everything with request time > 20 sec, if total amount of 
    active procs is > 120.
    
        apachetop --kill --mintime 10 --minprocs 120
    
    Find all procs that are sending data, sorted on number of procs per 
    site.
    
        pboxes -f app --ssh "apachetop --state W" | awk '{ print $13; }' |        sort | uniq -c | sort -n
    
    Emergency measure to keep a hammered box alive, The -E only sends 
    mail if the message body is non-empty
    
        while (sleep 5); 
        do
            apachetop --mintime 20 --minprocs 140 --kill |            mail -E -s "apache killer @ BU.com" gwillem@byte.nl; 
        done

