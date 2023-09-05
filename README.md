import numpy as np 
import pandas as pd 

mport os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))
        /kaggle/input/test-file/tested.csv


df = pd.read_csv("C:/Users/dell/Downloads/tested.csv")
df.head()

df.shape


df = df.drop(["PassengerId", "Ticket"], axis=1)

df["Title"] = df["Name"].apply(lambda x: x.split(",")[1].strip().split(" ")[0])
​
common_titles = ["Mr.", "Miss.", "Mrs."]
df["Title"] = [0 if x in common_titles else 1 for x in df["Title"]]
df["Cabin"] = [0 if str(x) == "nan" else 1 for x in df["Cabin"]]


embarked = pd.get_dummies(df["Embarked"])


df = pd.concat([df, embarked], axis=1)


df.head()


mean_mr = df[df["Name"].str.contains('Mr.', na=False)]['Age'].mean().round()
mean_miss = df[df["Name"].str.contains('Miss.', na=False)]['Age'].mean().round()
mean_mrs = df[df["Name"].str.contains('Mrs.', na=False)]['Age'].mean().round()
mean_master = df[df["Name"].str.contains('Master.', na=False)]['Age'].mean().round()
mean_dr = df[df["Name"].str.contains('Dr.', na=False)]['Age'].mean().round()
​
print("Mr: ", mean_mr)
print("Miss: ", mean_miss)
print("Mrs: ", mean_mrs)
print("Master: ", mean_master)
print("Dr: ", mean_dr)
Mr:  34.0
Miss:  22.0
Mrs:  39.0
Master:  7.0
Dr:  34.0


ages = {
    "Mr.": 34.0,
    "Miss.": 22.0,
    "Mrs.": 39.0,
    "Master.": 7.0,
    "Dr.": 34.0
}


def age(text):
    name = text[0]
    age = text[1]
    
    if pd.isnull(age):
        for k, v in ages.items():
            if k in name:
                return v
            
    else:
        return age


df['Age'] = df[['Name', 'Age']].apply(age, axis=1)


df.drop(["Name", "Embarked"], axis=1, inplace=True)
df.head()


df = df.dropna(how="any")


df["Survived"].value_counts().plot(kind="bar")


df["Pclass"].value_counts().plot(kind="bar")


df["Sex"].value_counts().plot(kind="pie", autopct="%.2f%%")


df.hist(figsize=(15, 15))


df["Sex"] = df["Sex"].map({
    "male": 0, "female": 1
})


df.head()
