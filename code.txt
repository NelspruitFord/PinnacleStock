javascript:(function() {
    // Define the raw GitHub URL of the CSV file
    var csvUrl = "https://raw.githubusercontent.com/NelspruitFord/PinnacleStock/main/PinnacleStockExport.csv";

    // Fetch the CSV file from the provided URL
    fetch(csvUrl)
        .then(response => response.text())
        .then(csv => {
            console.log("CSV file loaded successfully:");

            // Parse the CSV data
            var lines = csv.trim().split('\n'); // Trim any leading/trailing whitespace
            var headers = lines[0].split(',').map(header => header.replace(/"/g, ''));
            var stockNumberIndex = headers.indexOf("Stock Number");
            var vinIndex = headers.indexOf("VIN");
            var registrationDateIndex = headers.indexOf("Registration Date");
            var odometerIndex = headers.indexOf("Odometer");
            var colourIndex = headers.indexOf("Colour");
            var data = [];

            for (var i = 1; i < lines.length; i++) {
                var columns = lines[i].split(',').slice(0, 50); // Limit to first 50 fields
                if (columns.length >= 5) { // Ensure at least five columns for required fields
                    var obj = {};
                    obj["Stock Number"] = columns[stockNumberIndex].replace(/"/g, '');
                    obj["VIN"] = columns[vinIndex].replace(/"/g, '');
                    obj["Registration Date"] = columns[registrationDateIndex].replace(/"/g, '');
                    obj["Odometer"] = columns[odometerIndex].replace(/"/g, '');
                    obj["Colour"] = columns[colourIndex].replace(/"/g, '');
                    data.push(obj);
                } else {
                    console.warn(`Skipping line ${i + 1} due to insufficient data: ${lines[i]}`);
                }
            }

            console.log("Parsed CSV data with required fields:", data);

            // Example: Access and use 'data' as needed
            if (data.length > 0) {
                var firstEntry = data[0];
                console.log("First Entry - Stock Number:", firstEntry["Stock Number"]);
                console.log("First Entry - VIN:", firstEntry["VIN"]);
                console.log("First Entry - Registration Date:", firstEntry["Registration Date"]);
                console.log("First Entry - Odometer:", firstEntry["Odometer"]);
                console.log("First Entry - Colour:", firstEntry["Colour"]);

                // Optionally, you can perform further operations with 'data' here
            } else {
                console.warn("No data found in CSV.");
            }
        })
        .catch(error => console.error('Error fetching or parsing CSV:', error));
})();
