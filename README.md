import requests
from bs4 import BeautifulSoup
import pandas as pd

try:
    # Get the URL from user input
    url = input("Enter the website URL: ")

    # Send a GET request to the website
    response = requests.get(url)

    # Check the response status code
    if response.status_code == 200:
        # Create a BeautifulSoup object
        soup = BeautifulSoup(response.content, "html.parser")

        # Find the table containing the data
        table = soup.find("table")

        # Extract the table headers
        headers = [header.text.strip() for header in table.find_all("th")]

        # Extract the table rows
        rows = []
        for row in table.find_all("tr")[1:]:
            data = [cell.text.strip() for cell in row.find_all("td")]
            rows.append(data)

        # Create a pandas DataFrame from the extracted data
        df = pd.DataFrame(rows, columns=headers)

        # Write the DataFrame to an Excel file
        df.to_excel(r"C:\Users\hp\Downloads\population_data.xlsx", index=False)
        
        # Write the DataFrame to an CSV file
        df.to_csv(r"C:\Users\hp\Downloads\population_data.csv", index=False)

        print("Data scraped and saved to population_data.xlsx.")
    else:
        print("Error: The website returned a non-200 status code.")
except requests.exceptions.RequestException:
    print("Invalid URL. Please enter a valid website URL.")
