Firstly we will use here Putty for cli and also use winSCP for file transfer from windows to linux.

Now we need to download access.log file in windows and we will transer this file to linux using winSCP.
We found an log file in github and download it.
Then transfer that file to linux using WinSCP before transfer we should create a directory in linux.

**Create a working directory:**
```bash
mkdir logs
```

Then transfer that file to this directory. 

After transfer, Now we can see this file in the directory.
we can use following command to see
```bash
ls
```
also we can use following command to verify the file
```bash
ls -lh access.log
head -5 access.log
```

### 1: Now we want to see total requests in the log file. To see this we use 
```bash
wc -l access.log
```
**Explanation:**
- `wc` = word count command
- `-l` = count lines
- Each line = one request

### 2: Now we want to see how many unique IP address made requests?
**Step by Step approach:**
1.  **Extract just the IP addresses:**
```bash
awk '{print $1}' access.log | head -10
 ```
 **Explanation:**
   - `awk '{print $1}'` = print first field (IP address)
   - `| head -10` = show first 10 results

2. **Sort the IP addresses:**
   ```bash
   awk '{print $1}' access.log | sort | head -10
   ```
   
   **Explanation:**
   - `sort` = arrange in alphabetical/numerical order
   - This groups duplicate IPs together

3. **Remove duplicates:**
   ```bash
   awk '{print $1}' access.log | sort | uniq | head -10
   ```
   
   **Explanation:**
   - `uniq` = remove adjacent duplicate lines
   - Must be used after `sort`

4. **Count unique IPs:**
   ```bash
   awk '{print $1}' access.log | sort | uniq | wc -l
   ```

   
**Alternative (shortcut) (more efficient):**
```bash
awk '{print $1}' access.log | sort -u | wc -l
```

**Explanation:**
- `sort -u` = sort and remove duplicates in one step

### 3. Now we want to see top 10 most frequent IP addresses
**Command:**
```bash
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -10
```

**Explanation:**
1. `awk '{print $1}'` - Extract IP addresses
2. `sort` - Sort them
3. `uniq -c` - Count occurrences of each unique IP
4. `sort -rn` - Sort by count (reverse numerical)
5. `head -10` - Show top 10

### 4. Now we want to see how many 404 (not found) error occured
**Command:**
```bash
grep " 404 " access.log | wc -l
```
**Explanation:**
- `grep " 404 "` = search for lines containing " 404 " (with spaces to avoid matching 4040, etc.)
- `wc -l` = count matching lines

**Better approach (using awk):**
```bash
awk '$9 == 404' access.log | wc -l
```

**Explanation:**
- `$9 == 404` = match lines where 9th field (status code) equals 404

**Analyze all status codes:**
```bash
awk '{print $9}' access.log | sort | uniq -c | sort -rn
```

### 5. Now we want to see the top 5 requested URLs that resulted in a 404 error.
**Command:**
```bash
awk '$9 == 404 {print $7}' access.log | sort | uniq -c | sort -rn | head -5
```
**Explanation:**
1. `$9 == 404` - Filter for 404 errors
2. `{print $7}` - Extract the URL (7th field)
3. `sort | uniq -c` - Count occurrences
4. `sort -rn` - Sort by frequency
5. `head -5` - Top 5

### 6. Now we want to see how many requests were made by the user agent "Nikto"?
**Command:**
```bash
grep -i "nikto" access.log | wc -l
```
**Explanation:**
- `grep -i` = case-insensitive search
- Searches in the entire line (including User-Agent field)

**Extract full details:**
```bash
grep -i "nikto" access.log | head -5
```

**Get unique IPs using Nikto:**
```bash
grep -i "nikto" access.log | awk '{print $1}' | sort -u
```

### 7. Now we want to find potential SQL injection attempts in the log

**Common SQL injection patterns:**
- `' OR '1'='1`
- `UNION SELECT`
- `DROP TABLE`
- `; --` (SQL comment)
- `%27` (URL-encoded single quote)

**Commands:**

1. **Search for SQL keywords:**
   ```bash
   grep -iE "(union|select|insert|update|delete|drop|exec|script)" access.log | wc -l
   ```

   **Explanation:**
   - `-i` = case-insensitive
   - `-E` = extended regex
   - `|` = OR operator

2. **Search for encoded SQL:**
   ```bash
   grep -E "(%27|%20union|%20select)" access.log
   ```

3. **Extract suspicious requests:**
   ```bash
   grep -iE "(union|select)" access.log | awk '{print $1, $7}' | head -10
   ```

   **Output shows:** IP address and requested URL

4. **Count by IP:**
   ```bash
   grep -iE "(union|select)" access.log | awk '{print $1}' | sort | uniq -c | sort -rn
   ```

### 8. Now we want to find potential Cross-Site Scripting (XSS) attempts
**XSS patterns:**
- `<script>`
- `javascript:`
- `onerror=`
- `%3Cscript%3E` (URL-encoded)

**Command:**
```bash
grep -iE "(<script|javascript:|onerror=|%3Cscript)" access.log
```

**Count XSS attempts:**
```bash
grep -iE "(<script|javascript:|onerror=|%3Cscript)" access.log | wc -l
```

**Identify attackers:**
```bash
grep -iE "(<script|javascript:|onerror=)" access.log | awk '{print $1}' | sort | uniq -c | sort -rn
```

### 9. We want to find Directory Traversal attempts
**Traversal patterns:**
- `../`
- `..%2F` (URL-encoded)
- `....//`

**Command:**
```bash
grep -E "(\.\./|\.\.%2[Ff])" access.log
```

**Count attempts:**
```bash
grep -E "(\.\./|\.\.%2[Ff])" access.log | wc -l
```

### 10. Now we want to see hours had the most traffic
**Command:**
```bash
awk '{print $4}' access.log | cut -d: -f2 | sort | uniq -c | sort -rn
```

**Explanation:**
1. `awk '{print $4}'` - Extract timestamp field
2. `cut -d: -f2` - Extract hour from timestamp (split by `:`, take 2nd field)
3. `sort | uniq -c` - Count per hour
4. `sort -rn` - Sort by count

### 11. What HTTP methods are being used?
**Command:**
```bash
awk '{print $6}' access.log | tr -d '"' | sort | uniq -c | sort -rn
```

**Explanation:**
- `awk '{print $6}'` - Extract method (GET, POST, etc.)
- `tr -d '"'` - Remove quotes
- Count and sort

### 12. Extract all IP addresses that made POST requests
**Command:**
```bash
awk '$6 == "\"POST"' access.log | awk '{print $1}' | sort -u
```

**Or combined:**
```bash
awk '$6 == "\"POST" {print $1}' access.log | sort -u
```

**Get POST request details:**
```bash
awk '$6 == "\"POST" {print $1, $7, $9}' access.log | head -10
```

### 13. Find total bandwidth used
**Command:**
```bash
awk '{sum += $10} END {print sum/1024/1024 " MB"}' access.log
```

**Explanation:**
- `{sum += $10}` - Add up all bytes (field 10)
- `END {print sum/1024/1024}` - Convert to MB and print

**Bandwidth by IP:**
```bash
awk '{bytes[$1] += $10} END {for (ip in bytes) print ip, bytes[ip]/1024/1024 " MB"}' access.log | sort -k2 -rn | head -10
```


### 14. Creating a clean list of Unique IPs
**Command:**
```bash
awk '{print $1}' access.log | sort -u > unique_ips.txt
```

**Verify:**
```bash
wc -l unique_ips.txt
head unique_ips.txt
```

### 15. Replace the last octet of IP addresses with "XXX" for privacy
**Command:**
```bash
sed 's/\([0-9]\{1,3\}\.\)\{3\}[0-9]\{1,3\}/\1XXX/' access.log | head -5
```

**Simpler approach:**
```bash
sed 's/\.[0-9]*\( \)/\.XXX\1/' access.log | head -5
```

### 16. Creating a file with just requested URLs
**Command:**
```bash
awk '{print $7}' access.log | sort -u > urls.txt
```

**Filter for specific file types:**
```bash
awk '{print $7}' access.log | grep -E "\.(php|asp|jsp)$" | sort -u
```


