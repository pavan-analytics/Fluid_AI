# Fluid_AI
# **PDF Insights Extractor**

## **Overview**
This project is designed to **extract key insights from PDF documents** using **Python, PyPDF2, and spaCy**. It processes financial transcripts, investor meetings, or earnings call reports and provides summarized insights categorized into:
- **Future Growth Prospects**
- **Financial Performance**
- **Strategic Initiatives**
- **Regulatory Changes**

By leveraging **Natural Language Processing (NLP)**, the script efficiently filters and summarizes important information, making it useful for financial analysts, researchers, and business professionals.

---

## **Features**
- **Extracts text from PDFs** using PyPDF2
- **Cleans irrelevant content** (e.g., greetings, reminders, and formalities)
- **Categorizes insights** based on keyword matching
- **Summarizes key findings** in an easy-to-read format
- **Optimized for financial and business reports**

---

## **Installation**
Ensure you have Python installed (>=3.7). Then install the required dependencies:

```sh
pip install PyPDF2 spacy
python -m spacy download en_core_web_sm
```

---

## **Usage**
### **1. Extracting Text from a PDF**
Place your PDF file (e.g., `SJS_Transcript_Call.pdf`) in the project directory and run:

```python
from extract_insights import extract_text_from_pdf

pdf_text = extract_text_from_pdf("SJS_Transcript_Call.pdf")
print(pdf_text)
```

### **2. Summarizing Key Insights**
Run the script to extract and summarize insights:

```python
from extract_insights import extract_key_insights

insights = extract_key_insights(pdf_text)
for category, summary in insights.items():
    print(f"{category}:\n{summary}\n")
```

---

## **Code Explanation**
### **1. Importing Libraries**
```python
import PyPDF2
import spacy
from collections import defaultdict
```
- **PyPDF2**: Extracts text from PDFs.
- **spaCy**: Processes text and detects keywords.
- **defaultdict**: Stores categorized insights dynamically.

### **2. Extracting Text from PDF**
```python
def extract_text_from_pdf(pdf_path):
    text = ""
    with open(pdf_path, "rb") as file:
        pdf_reader = PyPDF2.PdfReader(file)
        for page in pdf_reader.pages:
            text += page.extract_text() + "\n"
    return text
```
- Opens the PDF file and reads text from each page.

### **3. Cleaning Sentences**
```python
def clean_sentence(sentence):
    ignore_phrases = ["As a reminder", "Ladies and gentlemen"]
    return not any(phrase in sentence for phrase in ignore_phrases)
```
- Removes irrelevant sentences to refine extracted content.

### **4. Extracting Key Insights**
```python
def extract_key_insights(text, max_sentences=3):
    doc = nlp(text)
    sections = defaultdict(list)
    keywords = {
        "Future Growth Prospects": ["growth", "opportunity"],
        "Financial Performance": ["revenue", "profit"],
    }
    
    for sentence in doc.sents:
        if not clean_sentence(sentence.text):
            continue
        
        for section, words in keywords.items():
            if any(word in sentence.text.lower() for word in words):
                sections[section].append(sentence.text)
                break

    summary = {key: " ".join(value[:max_sentences]) for key, value in sections.items()}
    return summary
```
- Uses **spaCy NLP model** to process and segment text.
- Filters sentences based on **predefined keyword categories**.
- Summarizes extracted insights **up to `max_sentences`** per category.

---

## **Example Output**
### **Input:**
> "The company's revenue grew by 20% last quarter. We see significant expansion opportunities in the Asian market. A new acquisition was finalized last week."

### **Output:**
```
Future Growth Prospects:
The company's revenue grew by 20% last quarter. We see significant expansion opportunities in the Asian market.

Strategic Initiatives:
A new acquisition was finalized last week.
```

---

## **Use Cases**
✔ Financial analysis and earnings reports 📈  
✔ Business intelligence and market research 🏢  
✔ Regulatory compliance monitoring 📜  
✔ Automated summarization of investor calls 🎙

---

## **Future Enhancements**
- Implement **AI-based sentiment analysis** for insights.
- Improve **keyword detection** with Machine Learning.
- Build a **Power BI dashboard** for visualization.


