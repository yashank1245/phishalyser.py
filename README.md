import requests
from urllib.parse import urlparse
import streamlit as st

def check_url_safety(url):
    phishing_blacklist = ["example-phishing.com", "malicious-site.net"]
    parsed_url = urlparse(url)
    domain = parsed_url.netloc
    
    if domain in phishing_blacklist:
        return "Unsafe: This URL is flagged as a phishing site."
    
    try:
        response = requests.get(url, timeout=5)
        if response.status_code == 200:
            return "Safe: The URL is accessible and not flagged."
        else:
            return "Warning: The URL returned a non-200 status code."
    except requests.exceptions.RequestException:
        return "Error: Unable to reach the URL. It may be unsafe."

# Streamlit UI
st.title("Phishalyser: URL Safety Checker")
url = st.text_input("Enter a URL to check:")
if st.button("Check URL"):
    if url:
        result = check_url_safety(url)
        st.write(result)
    else:
        st.write("Please enter a valid URL.")
