# IEEE-CIS-Fraud-Detection

მოცემული გვაქვს ტრანზაქციები და უნდა დავაფრედიქთოდ Fraud ტრანზაქციები. ერთ-ერთი გამოწვევა ისაა, რომ გვაქვს დაუბალანსსებელი დატა და რამენაირად უნდა მოვაგვაროთ ეს პრობლემა. თავიდან ვცადე Logistic Regression-ით დაწყება, მარა ძაან დიდიხანი ჭირდებოდა და კეგლის რამიც არ ყოფნიდა ამიტომ საცდელად გავუშვი XGBoost თავიდან. 

**XGBoost:**

**NAN მნიშვნელობების დამუშავება:
**
კატეგორიული ცვლადებს ამ შემთხვევაში ვავსებ მოდით, ნუმერიკალებს მედიანით. 
სხვა ქლინინგ მიდგომა პირველი მოდელისთვის არ გამიკეთებია, საცდელად გავუშვი და მაინტერესბდა რაას იზამდა. 

**Feature Engineering:
**გამოვიყენე როგორც WOE ენკოდინგი, ასევე One-Hot. Threshold-ად unique მნიშვნელობებისთვის 2 ავიღე ამჯერად, მარა 3-ზეც ვცდი. 

**Feature Selection:
**
კორელაციის ფილტრი და RFE გამოვიყენე. კორელაციის სრეშოლდი 80% მქონდა, RFE-დან 20 Feature დავიტოვე. 

საბოლოო ფაიფლაინში დავამატე სქეილერი და ანდერსემფლერი, სადაც sampling_strategy 0.5 ავიღე. თავიდან ამის გარეშე ვაპირებდი შედეგის ნახვას, მარა მემორიმ ვერ გაუძლო და ვერ გავუშვი. 

**Training:
**

Hyperparameter ოპტიმიზაცია: GridSearchCV და Optuna-ს გამოყენებაც ვცადე, მარა ორივემ მემორიზე გაჭედა ჯერჯერობით. თუ რამე წინსვლა იქნება ამ მხრივ დავწერ მერე. 

**MLflow Tracking
**
MLflow ექსპერიმენტის ბმული: https://dagshub.com/nkikn21/IEEE-CIS-Fraud-Detection.mlflow/#/experiments/2?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

ჩაწერილი მეტრიკები:
Precision - 0.14465000540949907
Recall - 0.44655978623914494
F1 - 0.21851761052545554
AUC - 0.7970027042779819.
ესენია პირვეელი მოდელის შედეგები, AUC ნორმალურია, მარა f1 საშინელია, ამიტო გავაუმჯობესებ და სხვა მიდგომებს დავამატებ, უკეთესს პრეპროცესინგს. 

მეორე მოდელში მცირე ცვლილებები განვახორციეელე, one-hot და woe-ს სრეშოლდი 3-ზე ავიღე, RFE-ის მერე 30 დავტოვე და ანდერსემფლერში ის მნიშვნელობა 0.8 ავიღე, კიდე ნალების მნიშვნელობა თუ 80%-ზე მეტი იყო გადავყარე ეგეთი Features და ნუ ცოტათი გაიზარდა F1-ც(0.23614581331283235) და  AUC-ც(0.8315836733554125). 

მესამე მოდელში RFE-დან 10 დავიტოვე და ანდერსემფლერი 0.2-ზე დავწიე, AUC(0.639924737630922), F1(0.11926441706838065), სავარაუდოდ ვერ ისწავლა კარგად, ცოტა Feature დავუტოვე და რაოდენობაც საგრძნობლად შევამცირე. 

მეოთხე მოდელში რანდომ სერჩით შევარჩიე learning_rate, max_depth, n_estimators, ოღონდ მემორის როარ გაეჭედა 3-3 წერტილი ავარჩევინე რაღაც შუაელდებიდან რენდომად. შედეგიც ყველაზე კარგი ამას ქონდა:
AUC - 0.8798971371828037, 
F1 - 0.4636678200692041.

**LightGBM:**

**NAN მნიშვნელობების დამუშავება:
**
კატეგორიული ცვლადებს ამ შემთხვევაში ვავსებ მოდით, ნუმერიკალებს მედიანით. ასევე ვდროფავ ქოლუმნებს, სადაც 80%-ზე მეტი ნან მნიშვნელობაა. 

**Feature Engineering:
**გამოვიყენე როგორც WOE ენკოდინგი, ასევე One-Hot. Threshold-ად unique მნიშვნელობებისთვის ავიღე 3. 

**Feature Selection:
**
კორელაციის ფილტრი და RFE გამოვიყენე. კორელაციის სრეშოლდი 80% მქონდა, RFE-დან 30 Feature დავიტოვე. 

საბოლოო ფაიფლაინში დავამატე სქეილერი და ანდერსემფლერი, სადაც sampling_strategy 0.4 ავიღე.  

**Training:
**

**MLflow Tracking
**
MLflow ექსპერიმენტის ბმული: https://dagshub.com/nkikn21/IEEE-CIS-Fraud-Detection.mlflow/#/experiments/5?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

ჩაწერილი მეტრიკები:
Precision - 0.27346303501945524
Recall - 0.5868403473613895
F1 - 0.3730756980571186
AUC - 0.8849056317788666.
ესენია პირვეელი მოდელის შედეგები LGBM-ისთვის, AUC კარგია, მარა f1-მაც მოიმატა მარა მაინც საკმაოდ ცუდი შედეგია.  


**Decision Tree:**

პრეპროცესინგის ნაწილში ქლინინგი და Feature Engineering ერთნაირი ყველგან თითქმის, Feature Selection-ში 30 დავიტოვე RFE-ს მერე და Undersampler-ის მნიშვნელობა 0.4 მქონდა. შედეგები არც ისე სახარბიელო იყო : 
Precision - 0.08019625334522748
Recall - 0.6005344021376086
F1 - 0.14149681278035728
AUC - 0.6751883520222712. ამიტომ შეცვალე ზედა მნიშვნელობები და ვნახავ გაუმჯობესდება თუ არა. 

მეორე მოდელში 10 Feature დავიტოვე და 0.6ზე ავწიე ანდერსემფლერი. შედეგები:
Precision - 0.06836621796053845
Recall - 0.4953239812959252
F1 - 0.12014907234869965
AUC - 0.6250881151566723. ოდნავ გაუარესდა ისედაც ცუდი შედეგი. ცოტა Feature როცა აქ რამდენიმე მოდელზე ვნახე უკვე და გამოჩნდა რო ვერ სწავლობს კარგად, ამიტომ 20-30-ს ფარგლებში ვტოვებ RFE-ს მერე. ანდერსემფლერიც თუ ძაან დაწიე მაშინაც ვერ სწავლობს, თუ საერთოდ არ დავწიე მემორიზე ქონდა პრობლემა და სადღაც 0.4-0.5-ებს ვიღებ და შედარებით კარგი შედეგები მააგზე ქონდა. 

**Random Forest:**
ზოგადად რენდომ ფორესტზე, გრადიენტ ბუსტინგზე და SVC-ზე ძაააან დიდი ხანი დაჭირდა ტრენინგს. თავიდან როგორც სხვებზე დავტოვე 20-30 Feature-ს დატოვებაზე გავუშვი, მარა იმდენი ხანი დაჭირდა რო 150-მდე გავზარდე, მაგრამ მაინც საათობით ჭირდებოდა დასატრენინგებლად. რენდომ ფორესტმა საბოლოოდ ძლივს დააბრუნა პასუხი და საკმაოდ ნორმალური პასუხიც დადო:
Precision - 0.3503694581280788
Recall - 0.5701402805611222
F1 - 0.43401983218916856
AUC - 0.8947972124558692. 
იმ ორმა კიდე ვერ დამიბრუნა პასუხი 2საათის მერეც კი, ამიტომ, რახან დროშიც ვეღარ ვასწრებდი მომიწია თავი დამენებებინა. 


**Submissions:**
LGBM : ![image](https://github.com/user-attachments/assets/739081a5-e65b-467c-8d45-e9191913b0b8)
XGBoost : ![image](https://github.com/user-attachments/assets/e690a405-c3d3-41b0-a333-0d22eea4f192)
Random Forest : ![image](https://github.com/user-attachments/assets/e9acb439-2a55-405d-8fec-d2c1977de2ce)
Decision Tree : ![image](https://github.com/user-attachments/assets/4b2dcd4c-651b-4f06-b84b-ede92634037b)



