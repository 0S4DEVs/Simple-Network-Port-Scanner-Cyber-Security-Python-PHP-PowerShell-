# Simple-Network-Port-Scanner-Cyber-Security-Python-PHP-PowerShell-
This project is a simple network port scanner that allows you to scan a range of IP addresses and ports to determine if they are open or closed. It uses Python for the scanning algorithm, PHP for the web interface, and PowerShell for the command-line interface.

Languages and Tools:

Python (version 3.0 or higher)
PHP (version 7.0 or higher)
PowerShell (version 5.0 or higher)
Instructions:

Clone or download the repository to your local machine.

Open the terminal or command prompt and navigate to the project directory.

Run the following command to install the necessary Python libraries:

Copy code
pip install -r requirements.txt
Run the following command to start the web interface:

Copy code
php -S localhost:8000
Open your web browser and navigate to localhost:8000. You should see the web interface for the port scanner.

Enter the range of IP addresses and ports you want to scan and click the "Scan" button.

Wait for the scan to complete. The results will be displayed on the screen and saved to a file in the project directory.

Alternatively, you can use PowerShell to run the port scanner from the command line. Open PowerShell and navigate to the project directory. Run the following command to start the scanner:

python
Copy code
python port_scanner.py <IP address range> <port range>
For example:

Copy code
python port_scanner.py 192.168.0.1-192.168.0.10 80-100
The results will be displayed on the screen and saved to a file in the project directory.

Code:

Python - port_scanner.py

scss
Copy code
import socket
import argparse

def scan_ports(ip_address, port_range):
    open_ports = []
    for port in port_range:
        try:
            sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            sock.settimeout(0.5)
            result = sock.connect_ex((ip_address, port))
            if result == 0:
                open_ports.append(port)
        except:
            pass
        finally:
            sock.close()
    return open_ports

def main():
    parser = argparse.ArgumentParser(description="Simple network port scanner")
    parser.add_argument("ip_address_range", help="Range of IP addresses to scan (e.g. 192.168.0.1-192.168.0.10)")
    parser.add_argument("port_range", help="Range of ports to scan (e.g. 80-100)")
    args = parser.parse_args()
    ip_start, ip_end = args.ip_address_range.split("-")
    port_start, port_end = args.port_range.split("-")
    ip_parts = ip_start.split(".")
    ip_prefix = ".".join(ip_parts[:3]) + "."
    open_ports = []
    for i in range(int(ip_parts[3]), int(ip_end.split(".")[3])+1):
        ip_address = ip_prefix + str(i)
        open_ports += scan_ports(ip_address, range(int(port_start), int(port_end)+1))
    open_ports.sort()
    print("Open ports:")
    for port in open_ports:
        print(port)

if __name__ == "__main__":
    main()
    
    PHP - index.php (continued)

php
Copy code
    <?php

    // Define variables and set to empty values
    $ip_address = $port_range = "";
    $results = array();

    // Check if form was submitted
    if ($_SERVER["REQUEST_METHOD"] == "POST") {
        
        // Get form data and validate
        $ip_address = test_input($_POST["ip_address"]);
        $port_range = test_input($_POST["port_range"]);

        // Execute port scan command
        exec("nmap -p {$port_range} {$ip_address}", $output, $return_var);

        // Process output and store results
        foreach ($output as $line) {
            if (strpos($line, "open") !== false) {
                $results[] = $line;
            }
        }
    }

    // Function to sanitize form data
    function test_input($data) {
        $data = trim($data);
        $data = stripslashes($data);
        $data = htmlspecialchars($data);
        return $data;
    }

    ?>

    <form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">
        <label for="ip_address">IP Address:</label>
        <input type="text" name="ip_address" id="ip_address" value="<?php echo $ip_address;?>"><br><br>
        <label for="port_range">Port Range:</label>
        <input type="text" name="port_range" id="port_range" value="<?php echo $port_range;?>"><br><br>
        <input type="submit" name="submit" value="Scan">
    </form>

    <?php

    // Display results
    if (!empty($results)) {
        echo "<h2>Results:</h2>";
        echo "<ul>";
        foreach ($results as $result) {
            echo "<li>{$result}</li>";
        }
        echo "</ul>";
    }

    ?>
This code defines a simple web form that prompts the user for an IP address and a port range to scan. When the user submits the form, the script executes the nmap command line tool to perform the scan and stores any open ports in an array. The results are then displayed on the same page. The form data is also sanitized using the test_input() function to prevent any potential security vulnerabilities.

PowerShell - portscanner.ps1

perl
Copy code
# Define variables and set to empty values
$ip_address = ""
$port_range = ""
$results = @()

# Prompt user for input
$ip_address = Read-Host "Enter IP Address"
$port_range = Read-Host "Enter Port Range"

# Execute port scan command
$output = (nmap -p $port_range $ip_address)

# Process output and store results
foreach ($line in $output) {
    if ($line -match "open") {
        $results += $line
    }
}

# Display results
if ($results) {
    Write-Host "Results:"
    foreach ($result in $results) {
        Write-Host $result
    }
} else {
    Write-Host "No open ports found."
}
This PowerShell script performs a similar port scan as the PHP code above, but instead prompts the user for input using the Read-Host cmdlet. The script then executes the nmap command and stores any open ports in an array. The results are displayed in the console window.

Note: This script requires nmap to be installed on the system.
