# **Resume Analyzer**

The project aims to develop a resume parser that extracts informationl like:  Name, Email, Contact Number, Technical Skills, Current Location, Work Experince, Current Designation at Current Company. The resume can be of the formats such as 
.docx, .doc, .pdf, .jpeg, .jpg, .png. The project will return the csv file with all the details extracted from the resume by uploading a zip file that contains all the resume.

***
## **Installation**

To run the resume parser, you will need to install the following libraries using pip:

_csv: pip install csv_

_re: pip install re_

_os: pip install os_

_spacy: pip install spacy_

_PyPDF2: pip install PyPDF2_

_pytesseract: pip install pytesseract_

_docx: pip install docx_

_textract: pip install textract_

_cv2: pip install opencv-python_

_datetime: pip install datetime_

In addition to these libraries, you also need to download the required language models for Spacy. Run the following commands to download the language models:

_English small model: python -m spacy download en_core_web_sm_

_English large model: python -m spacy download en_core_web_lg_

***

## **Usage**

1. Clone the repository or download the project files to your local machine.

2. Install the required libraries and download the language models as mentioned in the Installation section.

3. Open the Resume_parser folder and open the Database.py file in a Python IDE or text editor.

4. Modify the path of the zip file in the file zip_file_path variable.

5. A custom entity model has been trained using spacy. The *json* dataset is a custom entity dataset which contains the text of the resume and annotated entities. The model has been loaded into the index file using the function.
```
def load_model(resume_text):
    nlp = spacy.load("path_to_model")
    doc = nlp(resume_text)
    return doc
```
6. The scripts contains several functions that handle different aspects of the resume analyzing process. The file and the function definitions and their descriptions are provided below:

>### index.py

This file imports the *combine.py* file which returns location, work experience and current role at current company by taking the resume_text as input.

**for_each_file(resume_directory)** : 

This function is called in the file Database.py and takes the path of the directory where the resumes are unzipped and based on entension of the file, the text is extracted and then processed to return a list of all the resumes with the details extracted from the resume.

**extract_text_image(imagepath)** : 

Extracts text from an image.

**extract_text_docx(path)** : 


Extracts text from .docx extension files.

**extract_text__from_doc(path)** : 

Extracts text from .doc extension file.

**extract_text_pdf(filepath)** : 

Extracts text from .pdf extension file.

**push_entities(resume_text)** : 

A spacy english large model is loaded and one more entity called Skill is added to the entity_ruler using add_pipe feature of nlp. The labeled dataset used for the Skills is uploaded in the Training_dataset folder. Then a dictionary is returned with labels and recognized entity text as key-value pair from the resume_text. 

**model_test_name(resume_text)** : 

Returns the name from the text of the resume using the model. The fucntions loads the model and searched for the label "Name" and if found, returns the name. 

**extract_name(dict, resume_text)** : 

The spacy large model is used to find the NER entities and if there is an entity "Person" in the dicionary returned from the push-entities fucntion, it compares it with the name extracted from the model. If they are same, the name is post processed to remove any digits or speacial characters. If both are not same, then the name from the model is returned. 

**extract_email_address(resume_text)** : 

The functions uses regex to match the pattern of email and then return the found text.

**extract_phone(resume_text)** : 

The functions uses regex to match the pattern of phone number and then return the found text.

**extract_skills(dict)** : 

The functions takes the dictionary returned from the push_entities() fucntions and returns the recognized skills as a comma(,) separated string. 

**extract_gender(resume_text)** : 

The fucntion uses regular expression to check if there is "gender" explicitly mentioned in the resume and returns the immediate text after that or returns "Not-Specified".

**extract_work_experience(work, resume_text)** : 

This compares the work experience extracted from the combine.py file and the the work experience that the model gives. If the model is able to fetch the work experience, this function returns that value or else returns the other value.

**initialize_result_dict(resume_text, dict_entities, filepath)** : 

The function iniliazes a dictionary with all the fields as keys and their corresponding values. Then returns the dictionary for the resume processed. 

>### unzip_folder.py

**unzip(folder_path)** : 

This functions unzips the folder and extracts each file at the respective directory and then returns the path of that directory.

**head_tail(folder_path)** : 

Returns the head and tail of the given path.

>### Database.py 

This file imports the index file and the unzip_folder file. 
It calls the function _put_in_csv(zip_file_path)_.

**put_in_csv(zip_file_path)** : 

This fuctions takes the path of the zip file as parameter and calls unzip() from unzip_folder which returns the path of the directory where all the resumes are extracted. Then for_each_file() is called whcih takes the directory of resume files as input and returns a list of dictionary for extracted details from each resume. 
Then this functions writes this list in a csv file with the given headers and saves the output csv file at the respective path. 

7.  Run the Database.py file.

>### Combine.py
