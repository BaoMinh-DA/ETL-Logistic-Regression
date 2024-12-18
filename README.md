# ETL-Logistic-Regression
HR Analytics: Job Change of Data Scientists

from google.colab import drive
drive.mount('/content/drive')

import pandas as pd

Enrollies_data = pd.read_excel('/content/drive/MyDrive/bài tập/Enrollies.xlsx')

Enrollies_data.head()

Enrollies_data.info()

mode_gender = Enrollies_data['gender'].mode()[0]
Enrollies_data['gender'] = Enrollies_data['gender'].fillna(mode_gender)

Look at data the column full_name has the object type, let change to string

Enrollies_data['full_name'] = Enrollies_data['full_name'].astype('string')

asdas

Enrollies_data['city'] = Enrollies_data['city'].astype('category')
Enrollies_data['gender'] = Enrollies_data['gender'].astype('category')

Enrollies_data.info()

enrollies_education = pd.read_excel('/content/drive/MyDrive/bài tập/enrollies_education.xlsx')

enrollies_education.head()

enrollies_education.info()

enrollies_education['enrolled_university'] = enrollies_education['enrolled_university'].astype('category')
enrollies_education['education_level'] = enrollies_education['education_level'].astype('category')
enrollies_education['major_discipline'] = enrollies_education['major_discipline'].astype('category')

enrollies_education['enrolled_university'] = enrollies_education['enrolled_university'].str.lower()
enrollies_education['education_level'] = enrollies_education['education_level'].str.lower()
enrollies_education['major_discipline'] = enrollies_education['major_discipline'].str.lower()

enrollies_education.info()

enrollies_education['enrolled_university'] = enrollies_education['enrolled_university'].fillna('Missing')
enrollies_education['education_level'] = enrollies_education['education_level'].fillna('Missing')
enrollies_education['major_discipline'] = enrollies_education['major_discipline'].fillna('Missing')

enrollies_education.sample(10)

enrollies_education.info()

work_experience = pd.read_csv('/content/drive/MyDrive/bài tập/work_experience.csv')

work_experience.sample(10)

work_experience.info()

work_experience['company_size'] = work_experience['company_size'].str.lower()
work_experience['company_type'] = work_experience['company_type'].str.lower()
work_experience['last_new_job'] = work_experience['last_new_job'].str.lower()

mode_ex = work_experience['experience'].mode()[0]
work_experience['experience'] = work_experience['experience'].fillna(mode_ex)

work_experience['company_size'] = work_experience['company_size'].fillna('Missing')
work_experience['company_type'] = work_experience['company_type'].fillna('Missing')
work_experience['last_new_job'] = work_experience['last_new_job'].fillna('Missing')

work_experience['relevent_experience'] = work_experience['relevent_experience'].astype('category')
work_experience['company_size'] = work_experience['company_size'].astype('category')
work_experience['company_type'] = work_experience['company_type'].astype('category')
work_experience['last_new_job'] = work_experience['last_new_job'].astype('category')
work_experience['experience'] = work_experience['experience'].astype('category')

work_experience.info()

from sqlalchemy import create_engine
!pip install pymysql

engine = create_engine('mysql+pymysql://etl_practice:550814@112.213.86.31:3360/company_course')
training_hours = pd.read_sql_table('training_hours', con=engine)

training_hours.head()

training_hours.info()

url = 'https://sca-programming-school.github.io/city_development_index/index.html'
table = pd.read_html(url)
city_development_index = table[0]

city_development_index.head()

city_development_index.info()

employment = pd.read_sql_table('employment', con=engine)


employment.head()

# Path to the SQLite database
db_path = 'data_warehouse.db'

# Create an SQLAlchemy engine
engine = create_engine(f'sqlite:///{db_path}')

Enrollies_data.to_sql('Enrollies_data', con=engine, if_exists='replace',index=False)
enrollies_education.to_sql('enrollines_education', con=engine, if_exists='replace',index=False)
work_experience.to_sql('work_experience', con=engine, if_exists='replace',index=False)
training_hours.to_sql('training_hours', con=engine, if_exists='replace',index=False)
city_development_index.to_sql('city_development_index', con=engine, if_exists='replace',index=False)
employment.to_sql('employment', con=engine, if_exists='replace',index=False)
