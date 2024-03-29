<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-gH2yIJqKdNHPEq0n4Mqa/HGKIhSkIHeL5AyhkYV8i59U5AR6csBvApHHNl/vI1Bx" crossorigin="anonymous">
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>COGAAN Scanner</title>
</head>

<body>
    <h1 class="text-center">--------COGAAN--------</h1>
    <h1>Scan QR HTML</h1>

    <div style="display: flex; justify-content: center">
        <div id="my-qr-reader" style="width: 600px"></div>
    </div>

    <div id="you-qr-result"></div>

    <script src="https://unpkg.com/html5-qrcode"></script>
    <script>
        function domReady(fn) {
            if (
                document.readyState === "complete" ||
                document.readyState === "interactive"
            ) {
                setTimeout(fn, 1);
            } else {
                document.addEventListener("DOMContentLoaded", fn);
            }
        }

        domReady(function() {
            var scannedQRData = new Set(); // Set to store scanned QR data

            function downloadScannedData() {
                var dataToDownload = Array.from(scannedQRData).join('\n');
                var blob = new Blob([dataToDownload], { type: 'text/plain' });
                var url = window.URL.createObjectURL(blob);
                var a = document.createElement('a');
                a.href = url;
                a.download = 'scanned_qr_data.txt';
                document.body.appendChild(a);
                a.click();
                window.URL.revokeObjectURL(url);
                document.body.removeChild(a);
            }

            function onScanSuccess(decodeText, decodeResult) {
                if (!scannedQRData.has(decodeText)) { // Check if QR code is not already scanned
                    scannedQRData.add(decodeText); // Add scanned QR data to the set
                    // Display all scanned QR data in the div
                    document.getElementById("you-qr-result").textContent = Array.from(scannedQRData).join('\n');
                }
            }

            var htmlScanner = new Html5QrcodeScanner("my-qr-reader", {
                fps: 10,
                qrbox: 250,
            });

            htmlScanner.render(onScanSuccess);

            // Download button event listener
            document.getElementById('downloadDataButton').addEventListener('click', function() {
                downloadScannedData();
            });
        });
    </script>
    <!-- Button to download scanned data -->
    <button id="downloadDataButton" class="btn btn-primary">Download Scanned Data</button>
</body>

</html>
