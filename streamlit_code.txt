import streamlit as st
from streamlit_option_menu import option_menu
from joblib import load

# Load saved model
kidney_model = load('C:/Users/hp/OneDrive/Desktop/Multiple Disease Prediction System/saved models/kidney_model.joblib')

with st.sidebar:
    selected = option_menu('Chronic Kidney Disease Prediction System',
                           ['About', 'Kidney Disease Prediction', 'Precautions','Find Nearby Nephrologists'],
                           icons=['info-circle', 'activity', 'prescription2','hospital'],
                           default_index=0)
    
if (selected == 'About'):
    st.title('About this Application')

    # Project Description
    st.write("This web application is designed to help you get a preliminary assessment of your risk for Chronic Kidney Disease (CKD) using a machine learning model. The model is a Random Forest Classifier trained on a dataset of kidney disease patients.")
    # Risk Factors (optional)
    st.write("**Risk Factors for CKD:**")
    st.write("- Diabetes: High blood sugar levels can damage the blood vessels in your kidneys.")
    st.write("- High blood pressure: Uncontrolled high blood pressure can put a strain on your kidneys and damage them over time.")
    st.write("- Family history of kidney disease: Having a parent, sibling, or child with CKD increases your risk.")
    st.write("- Age (over 60): As you age, your kidneys naturally become less efficient.")
    st.write("- Other health conditions: Certain conditions, such as heart disease and obesity, can increase your risk of CKD.")
    

    # Symptoms (optional)
    st.write("**Early stages of CKD may not cause any symptoms. However, as the disease progresses, you may experience:**")
    st.write("- Frequent urination, especially at night")
    st.write("- Blood in your urine (hematuria)")
    st.write("- High blood pressure")
    st.write("- Loss of appetite")
    st.write("- Difficulty sleeping")
    st.write("- Feeling tired and weak")
    st.write("- Puffiness around your eyes or swelling in your ankles and feet")
    st.write("**If you experience any of these symptoms, please consult a healthcare professional for proper diagnosis and treatment.**") 

    # Stages of CKD (optional)
    st.write("**Stages of CKD:**")
    st.write("CKD is classified into five stages based on the estimated glomerular filtration rate (eGFR), which indicates how well your kidneys are filtering waste products from your blood.")
    st.write("- Stage 1: Mildly decreased kidney function")
    st.write("- Stage 2: Moderately decreased kidney function")
    st.write("- Stage 3: Moderately severely decreased kidney function")
    st.write("- Stage 4: Severely decreased kidney function")
    st.write("- Stage 5: Kidney failure (end-stage renal disease)")
    st.write("**It's important to note that CKD can progress at different rates for different people.**")


    # How it Works (optional)
    
    # Additional Resources
    st.write("**Additional Resources:**")
    st.write("- National Institutes of Health (NIH):")
    st.write("    * National Kidney Disease Education Program (NKDEP): [Link to NKDEP](https://www.niddk.nih.gov/health-information/kidney-disease/chronic-kidney-disease-ckd/managing)")
    st.write("    * National Institute of Diabetes and Digestive and Kidney Diseases (NIDDK): [Link to NIDDK](https://www.niddk.nih.gov/health-information/kidney-disease)")
    st.write("- Kidney Organizations:")
    st.write("    * American Kidney Fund (AKF): [Link to AKF](https://www.kidneyfund.org/)")
    st.write("    * National Kidney Foundation (NKF): [Link to NKF](https://www.kidney.org/)")
    st.write("- Medical Websites:")
    st.write("    * Mayo Clinic: Chronic Kidney Disease: [Link to Mayo Clinic CKD info](https://www.mayoclinic.org/diseases-conditions/chronic-kidney-disease/symptoms-causes/syc-20352501)")
    # st.write("    * WebMD: Chronic Kidney Disease [invalid URL removed]")  # Unreliable source, remove
    



if (selected == 'Kidney Disease Prediction'):
    # Page title
    st.title('Kidney Disease Prediction using ML')

    # Getting input data from the user
    col1, col2, col3 = st.columns(3)

    with col1:
        age = st.text_input('Age of the Person', key='age')  # Assign unique key for each input
        bp = st.text_input('Blood Pressure value', key='bp')
        sg = st.text_input('Specific Gravity value', key='sg')

    with col2:
        al = st.text_input('Albumin value', key='al')
        su = st.text_input('Sugar value', key='su')
        rbc = st.text_input('Red Blood Cells', key='rbc')

    with col3:
        pc = st.text_input('Pus Cell', key='pc')
        pcc = st.text_input('Pus Cell Clumps', key='pcc')
        ba = st.text_input('Bacteria', key='ba')

    with col1:
        bgr = st.text_input('Blood Glucose Random', key='bgr')
        bu = st.text_input('Blood Urea', key='bu')
        sc = st.text_input('Serum Creatinine', key='sc')

    with col2:
        sod = st.text_input('Sodium', key='sod')
        pot = st.text_input('Potassium', key='pot')
        hemo = st.text_input('Hemoglobin', key='hemo')

    with col3:
        pcv = st.text_input('Packed Cell Volume', key='pcv')
        wc = st.text_input('White Blood Cell Count', key='wc')
        rc = st.text_input('Red Blood Cell Count', key='rc')

    # Code for prediction
    kidney_diagnosis = ''

    # Creating a button for Prediction
    if st.button('Kidney Disease Test Result'):
        # Handle empty input and potential errors
        try:
            # Create a list to store user input (assuming all features are numerical)
            user_input = [float(st.session_state[x]) if st.session_state.get(x) else 0.0 for x in ['age', 'bp', 'sg', 'al', 'su', 'rbc', 'pc', 'pcc', 'ba', 'bgr', 'bu', 'sc', 'sod', 'pot', 'hemo', 'pcv', 'wc', 'rc']]

            # Wrap the user input in another list (matches model input format)
            user_input_list = [user_input]

            kidney_prediction = kidney_model.predict(user_input_list)

            if (kidney_prediction[0] == 1):
                kidney_diagnosis = 'Based on the results, there is a chance you may have Chronic Kidney Disease (CKD). To confirm this and get the best course of action, please consult a nephrologist. '
            else:
                kidney_diagnosis = 'The person does not have Chronic Kidney Disease.'

        except ValueError:
            st.error('Please enter valid numbers for all features.')

    st.success(kidney_diagnosis)

if (selected == 'Precautions'):
    st.title('Precautions to Reduce Risk of Chronic Kidney Disease (CKD)')

    # Introduction
    st.write("While there's no cure for CKD, certain lifestyle changes and precautions can help you manage the condition, slow its progression, and potentially prevent complications. Here are some key steps you can take:")

    # Diet (optional)
    st.write("**Diet:**")
    st.write("- **Limit salt intake:** High sodium intake can raise blood pressure and put a strain on your kidneys.")
    st.write("- **Control protein intake:** Consult your doctor about the recommended protein amount for your specific condition.")
    st.write("- **Maintain a healthy weight:** Being overweight or obese increases your risk of CKD and other health problems.")
    st.write("- **Choose a balanced diet:** Focus on fruits, vegetables, whole grains, and lean protein sources.")
    st.write("- **Limit sugary drinks and processed foods:** These can contribute to weight gain and other health issues.")

    # Lifestyle (optional)
    st.write("**Lifestyle:**")
    st.write("- **Exercise regularly:** Aim for at least 30 minutes of moderate-intensity exercise most days of the week.")
    st.write("- **Manage stress:** Chronic stress can worsen blood pressure and potentially impact kidney health.")
    st.write("- **Don't smoke:** Smoking reduces blood flow to the kidneys and increases your risk of CKD.")
    st.write("- **Limit alcohol consumption:** Excessive alcohol intake can raise blood pressure and contribute to other health problems.")
    st.write("- **Get regular checkups:** Schedule regular appointments with your doctor to monitor your kidney function and overall health.")

    # Medication (optional)
    st.write("**Medication:**")
    st.write("If you have diabetes or high blood pressure, it's crucial to take your medications as prescribed by your doctor to control these conditions and potentially slow CKD progression.")

if selected == 'Find Nearby Nephrologists':
    st.title('Find Nearby Kidney Hospitals or Nephrologists')
    
    # User enters their address (optional - removed)
    # address = st.text_input('Enter your address in Hyderabad (optional):')

    st.header('Famous Kidney Hospitals:')

    # Replace placeholders with links to relevant websites (ensure they are reputable sources)
    st.write('- Apollo Hospitals: [Link to Apollo Hospitals Nephrology department](https://www.apollohospitals.com/departments/nephrology.aspx)')
    st.write('- Gleneagles Global Hospitals: [Link to Gleneagles Global Hospitals Nephrology department](https://www.gleneagleshospitals.co.in/gleneagles-hospital-lakdi-ka-pul-hyderabad)')
    st.write('- Yashoda Hospitals: [Link to Yashoda Hospitals Nephrology department](https://www.yashodahospitals.com/specialities/nephrology/)')
    st.write('- Fortis Hospitals: [Link to Fortis Hospitals Nephrology department](https://www.fortishealthcare.com/)')
    st.header('**Nephrology Specialists in Hyderabad:**')
    st.write('- https://www.practo.com/hyderabad/nephrologist')

    # Additional resources (optional)
    st.header('**Additional Resources:**')
    st.write('- National Kidney Foundation: [Link to National Kidney Foundation](https://www.kidney.org/)')
    st.write('- American Kidney Fund: [Link to American Kidney Fund](https://www.kidneyfund.org/)')

