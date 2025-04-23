## **IEEE-CIS Fraud Detection**

ეს არის კლასიფიკაციის ამოცანა, რომელიც მიზნად ისახავს ყალბი ტრანზაქციების აღმოჩენას. dataset იყო დიდი და დაუბალანსებელი - ძირითადად შეიცავდა არაყალბ ტრანზაქციებს, მთლიანი მონაცემების მხოლოდ 0.035 ნაწილი იყო ყალბი. ჩემი მიდგომა იყო, რომ გამესუფთავებინა მონაცემები, შემევსო missing values და კატეგორიული სვეტები გადამეყვანა რიცხვითში. შემდეგ შევეცადე მიღებული feature-ებიდან ამომერჩია ყველაზე მნიშვნელოვანი სვეტები. რადგანაც მონაცემები დაუბალანსებელი იყო და თან ძალიან ბევრი, გადავწყვიტე UnderSampling გამეკეთებინა და შემდეგ გადამეცა მოდელისთვის.

**რეპოზიტორიის სტრუქტურა**

model_experiment_XGBoost.ipynb - Training using XGBoost
model_experiment_RandomForest.ipynb - Training using RandomForest
model_experiment_LogisticRegression.ipynb - Training using LogisticRegression


**Feature Engineering**

მოცემული იყო 2 ფაილი ტრეინინგისთვის - train_identity.csv და train_transaction.csv. ისინი გავაერთიანე ერთ dataset-ში left merge-ით TransactionId-ებზე(transaction იყო left).
Feature Engineering-ისთვის გამოვიყენე რამდენიმე მიდგომა, პირველ რიგში Nan მნიშვნელობები შევავსე მოდებით. 
შემდეგ კატეგორიული ცვლადები დავყავი 2 ნაწილად - იმის მიხედვით, თუ რამდენი შესაძლო მნიშვნელობის მიღება შეეძლო, ვიყენებდი woe ან one-hot encoding-ს. 
scaling-ისთვის გამოვიყენე StandardScaler.
ასევე train_transaction-ში იყო feature TransactionDT, რომელიც კონკრეტული დროის მომენტიდან გასულ დროს ასახავდა. ეს ინფორმაცია უფრო გასაგები და გამოყენებადი რომ ყოფილიყო, გადავწყვიტე შემეცვალა. 
ამ feature-იდან მივიღე რამდენიმე სხვა, მაგალითად ტრანზაქციის დღე, საათი, თვე, თვითონ ეს feature კი წავშალე.

**Feature Selection** 

საბოლოო pipeline-ში დავამატე correlation filter კლასი, რომელიც მაღალი კორელაციის სვეტებს აშორებდა, threshold-ად ავირჩიე 80%. 
იმისათვის, რომ გამერკვია, რომელი feature-ები იყო განსაკუთრებით მნიშვნელოვანი გამოვიყენე Shap(RF-სთვის rf.feature_importance). დავითვალე feature importance-ები, დავსორტე და ავირჩიე ყველაზე მნიშვნელოვანი feature-ები, რომლებიც ჯამში 95%-ს ფარავდნენ.

**Training**
