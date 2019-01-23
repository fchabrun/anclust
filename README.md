# anclust
Text annotation-based hierarchical clustering

Allows an easy implementation in R of hierarchical clustering based on text annotations. May be used to cluster genes, proteins or metabolites from high-throughput analyses.

See an example of use (R code) below.

```r
rm(list = ls())

# Create a dummy set of pathologies
# Each pathology will be annotated using wikipedia's (https://en.wikipedia.org/wiki/Main_Page) first paragraph.
pathologies <- list()

pathologies[["Chronic myelogenous leukemia"]] <- "Chronic myelogenous leukemia (CML), also known as chronic myeloid leukemia, is a cancer of the white blood cells. It is a form of leukemia characterized by the increased and unregulated growth of predominantly myeloid cells in the bone marrow and the accumulation of these cells in the blood. CML is a clonal bone marrow stem cell disorder in which a proliferation of mature granulocytes (neutrophils, eosinophils and basophils) and their precursors is found. It is a type of myeloproliferative neoplasm associated with a characteristic chromosomal translocation called the Philadelphia chromosome. CML is largely treated with targeted drugs called tyrosine-kinase inhibitors (TKIs) which have led to dramatic improved long-term survival rates since 2001. These drugs have revolutionized treatment of this disease and allow most patients to have a good quality of life when compared to the former chemotherapy drugs. In Western countries, CML accounts for 15–25% of all adult leukemias and 14% of leukemias overall (including the pediatric population, where CML is less common).[3]"
pathologies[["Chronic lymphocytic leukemia"]] <- "Chronic lymphocytic leukemia (CLL) is a type of cancer in which the bone marrow makes too many lymphocytes (a type of white blood cell).[2] Early on there are typically no symptoms.[2] Later non-painful lymph nodes swelling, feeling tired, fever, or weight loss for no clear reason may occur.[2] Enlargement of the spleen and anemia may also occur.[4][2] It typically worsens gradually.[2] Risk factors include having a family history of the disease.[2] Exposure to Agent Orange and certain insecticides might also be a risk.[4] CLL results in the buildup of B cell lymphocytes in the bone marrow, lymph nodes, and blood.[4] These cells do not function well and crowd out healthy blood cells.[2] CLL is divided into two main types: those with a mutated IGHV gene and those without.[4] Diagnosis is typically based on blood tests finding high numbers of mature lymphocytes and smudge cells.[5] Management of early disease is generally with watchful waiting.[5] Infections should more readily be treated with antibiotics.[4] In those with significant symptoms, chemotherapy or immunotherapy may be used.[4] The medications fludarabine, cyclophosphamide, and rituximab are typically the initial treatment in those who are otherwise healthy.[8] CLL affected about 904,000 people globally in 2015 and resulted in 60,700 deaths.[6][7] The disease most commonly occurs in people over the age of 50.[3] Males are affected more often than females.[3] It is much less common in people from Asia.[4] Five-year survival following diagnosis is approximately 83% in the United States.[3] It represents less than 1% of deaths from cancer.[7]"
pathologies[["Burkitt's lymphoma"]] <- "Burkitt lymphoma is a cancer of the lymphatic system, particularly B lymphocytes found in the germinal center. It is named after Denis Parsons Burkitt, a surgeon who first described the disease in 1958 while working in equatorial Africa.[1][2] The overall cure rate for Burkitt's lymphoma in developed countries is about 90%, but worse in low-income countries. Burkitt's lymphoma is uncommon in adults, where it has a worse prognosis.[3]"
pathologies[["Hodgkin's lymphoma"]] <- "Hodgkin's lymphoma (HL) is a type of lymphoma which is generally believed to result from white blood cells of the lymphocyte kind.[8] Symptoms may include fever, night sweats, and weight loss.[2] Often there will be non-painful enlarged lymph nodes in the neck, under the arm, or in the groin.[2] Those affected may feel tired or be itchy.[2] About half of cases of Hodgkin's lymphoma are due to Epstein–Barr virus (EBV).[3] Other risk factors include a family history of the condition and having HIV/AIDS.[2][3] There are two major types of Hodgkin lymphoma: classical Hodgkin lymphoma and nodular lymphocyte-predominant Hodgkin lymphoma.[5] Diagnosis is by finding Hodgkin's cells such as multinucleated Reed–Sternberg cells (RS cells) in lymph nodes.[2] Hodgkin lymphoma may be treated with chemotherapy, radiation therapy, and stem cell transplant.[4] The choice of treatment often depends on how advanced the cancer has become and whether or not it has favorable features.[4] In early disease, a cure is often possible.[9] The percentage of people who survive five years in the United States is 86%.[5] For those under the age of 20, rates of survival are 97%.[10] Radiation and some chemotherapy drugs, however, increase the risk of other cancers, heart disease, or lung disease over the subsequent decades.[9] In 2015 about 574,000 people had Hodgkin's lymphoma, and 23,900 died.[6][7] In the United States, 0.2% of people are affected at some point in their life.[5] The most common age of diagnosis is between 20 and 40 years old.[5] It was named after the English physician Thomas Hodgkin, who first described the condition in 1832.[9][11]"

pathologies[["Wiskott–Aldrich syndrome"]] <- "Wiskott–Aldrich syndrome (WAS) is a rare X-linked recessive disease characterized by eczema, thrombocytopenia (low platelet count), immune deficiency, and bloody diarrhea (secondary to the thrombocytopenia).[1] It is also sometimes called the eczema-thrombocytopenia-immunodeficiency syndrome in keeping with Aldrich's original description in 1954.[2] The WAS-related disorders of X-linked thrombocytopenia (XLT) and X-linked congenital neutropenia (XLN) may present similar but less severe symptoms and are caused by mutations of the same gene."
pathologies[["Systemic lupus erythematosus"]] <- "Systemic lupus erythematosus (SLE), also known simply as lupus, is an autoimmune disease in which the body's immune system mistakenly attacks healthy tissue in many parts of the body.[1] Symptoms vary between people and may be mild to severe.[1] Common symptoms include painful and swollen joints, fever, chest pain, hair loss, mouth ulcers, swollen lymph nodes, feeling tired, and a red rash which is most commonly on the face.[1] Often there are periods of illness, called flares, and periods of remission during which there are few symptoms.[1] The cause of SLE is not clear.[1] It is thought to involve genetics together with environmental factors.[4] Among identical twins, if one is affected there is a 24% chance the other one will be as well.[1] Female sex hormones, sunlight, smoking, vitamin D deficiency, and certain infections, are also believed to increase the risk.[4] The mechanism involves an immune response by autoantibodies against a person's own tissues.[1] These are most commonly anti-nuclear antibodies and they result in inflammation.[1] Diagnosis can be difficult and is based on a combination of symptoms and laboratory tests.[1] There are a number of other kinds of lupus erythematosus including discoid lupus erythematosus, neonatal lupus, and subacute cutaneous lupus erythematosus.[1] There is no cure for SLE.[1] Treatments may include NSAIDs, corticosteroids, immunosuppressants, hydroxychloroquine, and methotrexate.[1] Alternative medicine has not been shown to affect the disease.[1] Life expectancy is lower among people with SLE.[5] SLE significantly increases the risk of cardiovascular disease with this being the most common cause of death.[4] With modern treatment about 80% of those affected survive more than 15 years.[3] Women with lupus have pregnancies that are higher risk but are mostly successful.[1] Rate of SLE varies between countries from 20 to 70 per 100,000.[2] Women of childbearing age are affected about nine times more often than men.[4] While it most commonly begins between the ages of 15 and 45, a wide range of ages can be affected.[1][2] Those of African, Caribbean, and Chinese descent are at higher risk than white people.[4][2] Rates of disease in the developing world are unclear.[6] Lupus is Latin for \"wolf\": the disease was so-named in the 13th century as the rash was thought to appear like a wolf's bite.[7]"
pathologies[["Rheumatoid arthritis"]] <- "Rheumatoid arthritis (RA) is a long-term autoimmune disorder that primarily affects joints.[1] It typically results in warm, swollen, and painful joints.[1] Pain and stiffness often worsen following rest.[1] Most commonly, the wrist and hands are involved, with the same joints typically involved on both sides of the body.[1] The disease may also affect other parts of the body.[1] This may result in a low red blood cell count, inflammation around the lungs, and inflammation around the heart.[1] Fever and low energy may also be present.[1] Often, symptoms come on gradually over weeks to months.[2] While the cause of rheumatoid arthritis is not clear, it is believed to involve a combination of genetic and environmental factors.[1] The underlying mechanism involves the body's immune system attacking the joints.[1] This results in inflammation and thickening of the joint capsule.[1] It also affects the underlying bone and cartilage.[1] The diagnosis is made mostly on the basis of a person's signs and symptoms.[2] X-rays and laboratory testing may support a diagnosis or exclude other diseases with similar symptoms.[1] Other diseases that may present similarly include systemic lupus erythematosus, psoriatic arthritis, and fibromyalgia among others.[2] The goals of treatment are to reduce pain, decrease inflammation, and improve a person's overall functioning.[5] This may be helped by balancing rest and exercise, the use of splints and braces, or the use of assistive devices.[1] Pain medications, steroids, and NSAIDs are frequently used to help with symptoms.[1] Disease-modifying antirheumatic drugs (DMARDs), such as hydroxychloroquine and methotrexate, may be used to try to slow the progression of disease.[1] Biological DMARDs may be used when disease does not respond to other treatments.[6] However, they may have a greater rate of adverse effects.[7] Surgery to repair, replace, or fuse joints may help in certain situations.[1] Most alternative medicine treatments are not supported by evidence.[8][9] RA affects about 24.5 million people as of 2015.[10] This is between 0.5 and 1% of adults in the developed world with 5 and 50 per 100,000 people newly developing the condition each year.[3] Onset is most frequent during middle age and women are affected 2.5 times as frequently as men.[1] In 2013, it resulted in 38,000 deaths up from 28,000 deaths in 1990.[11] The first recognized description of RA was made in 1800 by Dr. Augustin Jacob Landré-Beauvais (1772–1840) of Paris.[12] The term rheumatoid arthritis is based on the Greek for watery and inflamed joints.[13]"
pathologies[["Psoriasis"]] <- "Psoriasis is a long-lasting autoimmune disease characterized by patches of abnormal skin.[6] These skin patches are typically red, itchy, and scaly.[3] Psoriasis varies in severity from small, localized patches to complete body coverage.[3] Injury to the skin can trigger psoriatic skin changes at that spot, which is known as the Koebner phenomenon.[9] There are five main types of psoriasis: plaque, guttate, inverse, pustular, and erythrodermic.[6] Plaque psoriasis, also known as psoriasis vulgaris, makes up about 90 percent of cases.[4] It typically presents as red patches with white scales on top.[4] Areas of the body most commonly affected are the back of the forearms, shins, navel area, and scalp.[4] Guttate psoriasis has drop-shaped lesions.[6] Pustular psoriasis presents as small non-infectious pus-filled blisters.[10] Inverse psoriasis forms red patches in skin folds.[6] Erythrodermic psoriasis occurs when the rash becomes very widespread, and can develop from any of the other types.[4] Fingernails and toenails are affected in most people with psoriasis at some point in time.[4] This may include pits in the nails or changes in nail color.[4] Psoriasis is generally thought to be a genetic disease that is triggered by environmental factors.[3] In twin studies, identical twins are three times more likely to be affected compared to non-identical twins. This suggests that genetic factors predispose to psoriasis.[4] Symptoms often worsen during winter and with certain medications, such as beta blockers or NSAIDs.[4] Infections and psychological stress can also play a role.[3][6] Psoriasis is not contagious.[4] The underlying mechanism involves the immune system reacting to skin cells.[4] Diagnosis is typically based on the signs and symptoms.[4] There is no cure for psoriasis; however, various treatments can help control the symptoms.[4] These treatments include steroid creams, vitamin D3 cream, ultraviolet light and immune system suppressing medications, such as methotrexate.[6] About 75 percent of cases can be managed with creams alone.[4] The disease affects two to four percent of the population.[8] Men and women are affected with equal frequency.[6] The disease may begin at any age, but typically starts in adulthood.[5] Psoriasis is associated with an increased risk of psoriatic arthritis, lymphomas, cardiovascular disease, Crohn's disease and depression.[4] Psoriatic arthritis affects up to 30 percent of individuals with psoriasis.[10]"

pathologies[["Tuberculosis"]] <- "Tuberculosis (TB) is an infectious disease usually caused by the bacterium Mycobacterium tuberculosis (MTB).[1] Tuberculosis generally affects the lungs, but can also affect other parts of the body.[1] Most infections do not have symptoms, in which case it is known as latent tuberculosis.[1] About 10% of latent infections progress to active disease which, if left untreated, kills about half of those infected.[1] The classic symptoms of active TB are a chronic cough with blood-containing sputum, fever, night sweats, and weight loss.[1] The historical term \"consumption\" came about due to the weight loss.[4] Infection of other organs can cause a wide range of symptoms.[5] Tuberculosis is spread through the air when people who have active TB in their lungs cough, spit, speak, or sneeze.[1][6] People with latent TB do not spread the disease.[1] Active infection occurs more often in people with HIV/AIDS and in those who smoke.[1] Diagnosis of active TB is based on chest X-rays, as well as microscopic examination and culture of body fluids.[7] Diagnosis of latent TB relies on the tuberculin skin test (TST) or blood tests.[7] Prevention of TB involves screening those at high risk, early detection and treatment of cases, and vaccination with the bacillus Calmette-Guérin (BCG) vaccine.[8][9][10] Those at high risk include household, workplace, and social contacts of people with active TB.[10] Treatment requires the use of multiple antibiotics over a long period of time.[1] Antibiotic resistance is a growing problem with increasing rates of multiple drug-resistant tuberculosis (MDR-TB) and extensively drug-resistant tuberculosis (XDR-TB).[1] Presently, one-third of the world's population is thought to be infected with TB.[1] New infections occur in about 1% of the population each year.[11] In 2016, there were more than 10 million cases of active TB which resulted in 1.3 million deaths.[3] This makes it the number one cause of death from an infectious disease.[3] More than 95% of deaths occurred in developing countries, and more than 50% in India, China, Indonesia, Pakistan, and the Philippines.[3] The number of new cases each year has decreased since 2000.[1] About 80% of people in many Asian and African countries test positive while 5–10% of people in the United States population test positive by the tuberculin test.[12] Tuberculosis has been present in humans since ancient times.[13]"
pathologies[["Endocarditis"]] <- "Endocarditis is an inflammation of the inner layer of the heart, the endocardium. It usually involves the heart valves. Other structures that may be involved include the interventricular septum, the chordae tendineae, the mural endocardium, or the surfaces of intracardiac devices. Endocarditis is characterized by lesions, known as vegetations, which is a mass of platelets, fibrin, microcolonies of microorganisms, and scant inflammatory cells.[1] In the subacute form of infective endocarditis, the vegetation may also include a center of granulomatous tissue, which may fibrose or calcify.[2] There are several ways to classify endocarditis. The simplest classification is based on cause: either infective or non-infective, depending on whether a microorganism is the source of the inflammation or not. Regardless, the diagnosis of endocarditis is based on clinical features, investigations such as an echocardiogram, and blood cultures demonstrating the presence of endocarditis-causing microorganisms. Signs and symptoms include fever, chills, sweating, malaise, weakness, anorexia, weight loss, splenomegaly, flu-like feeling, cardiac murmur, heart failure, petechia of anterior trunk, Janeway's lesions, etc."
pathologies[["Typhoid fever"]] <- "Typhoid fever, also known simply as typhoid, is a bacterial infection due to Salmonella typhi that causes symptoms.[3] Symptoms may vary from mild to severe and usually begin six to thirty days after exposure.[1][2] Often there is a gradual onset of a high fever over several days.[1] Weakness, abdominal pain, constipation, and headaches also commonly occur.[2][6] Diarrhea is uncommon and vomiting is not usually severe.[6] Some people develop a skin rash with rose colored spots.[2] In severe cases there may be confusion.[6] Without treatment, symptoms may last weeks or months.[2] Other people may carry the bacterium without being affected; however, they are still able to spread the disease to others.[4] Typhoid fever is a type of enteric fever along with paratyphoid fever.[3] The cause is the bacterium Salmonella Typhi, also known as Salmonella enterica serotype Typhi, growing in the intestines and blood.[2][6] Typhoid is spread by eating or drinking food or water contaminated with the feces of an infected person.[4] Risk factors include poor sanitation and poor hygiene.[3] Those who travel to the developing world are also at risk[6] and only humans can be infected.[4] Diagnosis is by either culturing the bacteria or detecting the bacterium's DNA in the blood, stool, or bone marrow.[2][3][5] Culturing the bacterium can be difficult.[10] Bone marrow testing is the most accurate.[5] Symptoms are similar to that of many other infectious diseases.[6] Typhus is a different disease.[11] A typhoid vaccine can prevent about 30% to 70% of cases during the first two years.[7] The vaccine may have some effect for up to seven years.[3] It is recommended for those at high risk or people traveling to areas where the disease is common.[4] Other efforts to prevent the disease include providing clean drinking water, good sanitation, and handwashing.[2][4] Until it has been confirmed that an individual's infection is cleared, the individual should not prepare food for others.[2] Treatment of disease is with antibiotics such as azithromycin, fluoroquinolones or third generation cephalosporins.[3] Resistance to these antibiotics has been developing, which has made treatment of the disease more difficult.[3] In 2015, there were 12.5 million new cases worldwide.[8] The disease is most common in India.[3] Children are most commonly affected.[3][4] Rates of disease decreased in the developed world in the 1940s as a result of improved sanitation and use of antibiotics to treat the disease.[4] Each year in the United States, about 400 cases are reported and it is estimated that the disease occurs in about 6,000 people.[6][12] In 2015, it resulted in about 149,000 deaths worldwide – down from 181,000 in 1990 (about 0.3% of the global total).[9][13] The risk of death may be as high as 20% without treatment.[4] With treatment, it is between 1 and 4%.[3][4] The name typhoid means \"resembling typhus\" due to the similarity in symptoms.[14]"
pathologies[["Typhus"]] <- "Typhus, also known as typhus fever, is a group of infectious diseases that include epidemic typhus, scrub typhus and murine typhus.[1] Common symptoms include fever, headache, and a rash.[1] Typically these begin one to two weeks after exposure.[2] The diseases are caused by specific types of bacterial infection.[1] Epidemic typhus is due to Rickettsia prowazekii spread by body lice, scrub typhus is due to Orientia tsutsugamushi spread by chiggers, and murine typhus is due to Rickettsia typhi spread by fleas.[1] There is no commercially available vaccine.[3][4][5] Prevention is by reducing exposure to the organisms that spread the disease.[3][5][4] Treatment is with the antibiotic doxycycline.[2] Epidemic typhus generally occurs in outbreaks when poor sanitary conditions and crowding are present.[6] While once common, it is now rare.[3] Scrub typhus occurs in Southeast Asia, Japan, and northern Australia.[4] Murine typhus occurs in tropical and subtropical areas of the world.[5] Typhus has been described since at least 1528.[7] The name comes from the Greek typhus (τύφος) meaning hazy, describing the state of mind of those infected.[7] While \"typhoid\" means \"typhus-like\", typhus and typhoid fever are distinct diseases caused by different types of bacteria.[8]"

pathologies[["Diabetes mellitus type 1"]] <- "Diabetes mellitus type 1, also known as type 1 diabetes, is a form of diabetes mellitus in which not enough insulin is produced.[4] This results in high blood sugar levels in the body.[1] The classical symptoms are frequent urination, increased thirst, increased hunger, and weight loss.[4] Additional symptoms may include blurry vision, feeling tired, and poor healing.[2] Symptoms typically develop over a short period of time.[1] The cause of type 1 diabetes is unknown.[4] However, it is believed to involve a combination of genetic and environmental factors.[1] Risk factors include having a family member with the condition.[5] The underlying mechanism involves an autoimmune destruction of the insulin-producing beta cells in the pancreas.[2] Diabetes is diagnosed by testing the level of sugar or A1C in the blood.[5][7] Type 1 diabetes can be distinguished from type 2 by testing for the presence of autoantibodies.[5] There is no known way to prevent type 1 diabetes.[4] Treatment with insulin is required for survival.[1] Insulin therapy is usually given by injection just under the skin but can also be delivered by an insulin pump.[9] A diabetic diet and exercise are important parts of management.[2] Untreated, diabetes can cause many complications.[4] Complications of relatively rapid onset include diabetic ketoacidosis and nonketotic hyperosmolar coma.[5] Long-term complications include heart disease, stroke, kidney failure, foot ulcers and damage to the eyes.[4] Furthermore, complications may arise from low blood sugar caused by excessive dosing of insulin.[5] Type 1 diabetes makes up an estimated 5–10% of all diabetes cases.[8] The number of people affected globally is unknown, although it is estimated that about 80,000 children develop the disease each year.[5] Within the United States the number of people affected is estimated at one to three million.[5][10] Rates of disease vary widely with approximately 1 new case per 100,000 per year in East Asia and Latin America and around 30 new cases per 100,000 per year in Scandinavia and Kuwait.[11][12] It typically begins in children and young adults.[1]"
pathologies[["Diabetes mellitus type 2"]] <- "Diabetes mellitus type 2 (also known as type 2 diabetes) is a long-term metabolic disorder that is characterized by high blood sugar, insulin resistance, and relative lack of insulin.[6] Common symptoms include increased thirst, frequent urination, and unexplained weight loss.[3] Symptoms may also include increased hunger, feeling tired, and sores that do not heal.[3] Often symptoms come on slowly.[6] Long-term complications from high blood sugar include heart disease, strokes, diabetic retinopathy which can result in blindness, kidney failure, and poor blood flow in the limbs which may lead to amputations.[1] The sudden onset of hyperosmolar hyperglycemic state may occur; however, ketoacidosis is uncommon.[4][5] Type 2 diabetes primarily occurs as a result of obesity and lack of exercise.[1] Some people are more genetically at risk than others.[6] Type 2 diabetes makes up about 90% of cases of diabetes, with the other 10% due primarily to diabetes mellitus type 1 and gestational diabetes.[1] In diabetes mellitus type 1 there is a lower total level of insulin to control blood glucose, due to an autoimmune induced loss of insulin-producing beta cells in the pancreas.[12][13] Diagnosis of diabetes is by blood tests such as fasting plasma glucose, oral glucose tolerance test, or glycated hemoglobin (A1C).[3] Type 2 diabetes is partly preventable by staying a normal weight, exercising regularly, and eating properly.[1] Treatment involves exercise and dietary changes.[1] If blood sugar levels are not adequately lowered, the medication metformin is typically recommended.[7][14] Many people may eventually also require insulin injections.[9] In those on insulin, routinely checking blood sugar levels is advised; however, this may not be needed in those taking pills.[15] Bariatric surgery often improves diabetes in those who are obese.[8][16] Rates of type 2 diabetes have increased markedly since 1960 in parallel with obesity.[17] As of 2015 there were approximately 392 million people diagnosed with the disease compared to around 30 million in 1985.[11][18] Typically it begins in middle or older age,[6] although rates of type 2 diabetes are increasing in young people.[19][20] Type 2 diabetes is associated with a ten-year-shorter life expectancy.[10] Diabetes was one of the first diseases described.[21] The importance of insulin in the disease was determined in the 1920s.[22]"
pathologies[["Hypertension"]] <- "Hypertension (HTN or HT), also known as high blood pressure (HBP), is a long-term medical condition in which the blood pressure in the arteries is persistently elevated.[10] High blood pressure usually does not cause symptoms.[1] Long-term high blood pressure, however, is a major risk factor for coronary artery disease, stroke, heart failure, atrial fibrillation, peripheral vascular disease, vision loss, chronic kidney disease, and dementia.[2][3][4][11] High blood pressure is classified as either primary (essential) high blood pressure or secondary high blood pressure.[5] About 90–95% of cases are primary, defined as high blood pressure due to nonspecific lifestyle and genetic factors.[5][6] Lifestyle factors that increase the risk include excess salt in the diet, excess body weight, smoking, and alcohol use.[1][5] The remaining 5–10% of cases are categorized as secondary high blood pressure, defined as high blood pressure due to an identifiable cause, such as chronic kidney disease, narrowing of the kidney arteries, an endocrine disorder, or the use of birth control pills.[5] Blood pressure is expressed by two measurements, the systolic and diastolic pressures, which are the maximum and minimum pressures, respectively.[1] For most adults, normal blood pressure at rest is within the range of 100–130 millimeters mercury (mmHg) systolic and 60–80 mmHg diastolic.[7][12] For most adults, high blood pressure is present if the resting blood pressure is persistently at or above 130/90 or 140/90 mmHg.[5][7] Different numbers apply to children.[13] Ambulatory blood pressure monitoring over a 24-hour period appears more accurate than office-based blood pressure measurement.[5][10] Lifestyle changes and medications can lower blood pressure and decrease the risk of health complications.[8] Lifestyle changes include weight loss, decreased salt intake, physical exercise, and a healthy diet.[5] If lifestyle changes are not sufficient then blood pressure medications are used.[8] Up to three medications can control blood pressure in 90% of people.[5] The treatment of moderately high arterial blood pressure (defined as >160/100 mmHg) with medications is associated with an improved life expectancy.[14] The effect of treatment of blood pressure between 130/80 mmHg and 160/100 mmHg is less clear, with some reviews finding benefit[7][15][16] and others finding unclear benefit.[17][18][19] High blood pressure affects between 16 and 37% of the population globally.[5] In 2010 hypertension was believed to have been a factor in 18% of all deaths (9.4 million globally).[9]"
pathologies[["Angina"]] <- "Angina, also known as angina pectoris, is chest pain or pressure, usually due to not enough blood flow to the heart muscle. Angina is usually due to obstruction or spasm of the coronary arteries.[1] Other causes include anemia, abnormal heart rhythms and heart failure. The main mechanism of coronary artery obstruction is an atherosclerosis. The term derives from the Latin angere (\"to strangle\") and pectus (\"chest\"), and can therefore be translated as \"a strangling feeling in the chest\". There is a weak relationship between severity of pain and degree of oxygen deprivation in the heart muscle (i.e., there can be severe pain with little or no risk of a myocardial infarction (heart attack) and a heart attack can occur without pain). In some cases, angina can be quite severe, and in the early 20th century this was a known sign of impending death.[2] However, given current medical therapies, the outlook has improved substantially. People with an average age of 62 years, who have moderate to severe degrees of angina (grading by classes II, III, and IV) have a 5-year survival rate of approximately 92%.[3] Worsening angina attacks, sudden-onset angina at rest, and angina lasting more than 15 minutes are symptoms of unstable angina (usually grouped with similar conditions as the acute coronary syndrome). As these may precede a heart attack, they require urgent medical attention and are, in general, treated in similar fashion to myocardial infarction."
pathologies[["Cardiomyopathy"]] <- "Cardiomyopathy is a group of diseases that affect the heart muscle.[8] Early on there may be few or no symptoms.[1] Some people may have shortness of breath, feel tired, or have swelling of the legs due to heart failure.[1] An irregular heart beat may occur as well as fainting.[1] Those affected are at an increased risk of sudden cardiac death.[2] Types of cardiomyopathy include hypertrophic cardiomyopathy, dilated cardiomyopathy, restrictive cardiomyopathy, arrhythmogenic right ventricular dysplasia, and takotsubo cardiomyopathy (broken heart syndrome).[3] In hypertrophic cardiomyopathy the heart muscle enlarges and thickens.[3] In dilated cardiomyopathy the ventricles enlarge and weaken.[3] In restrictive cardiomyopathy the ventricle stiffens.[3] The cause is frequently unknown.[4] Hypertrophic cardiomyopathy usually is inherited, while dilated cardiomyopathy is inherited in a third of cases.[4] Dilated cardiomyopathy may also result from alcohol, heavy metals, coronary heart disease, cocaine use, and viral infections.[4] Restrictive cardiomyopathy may be caused by amyloidosis, hemochromatosis, and some cancer treatments.[4] Broken heart syndrome is caused by extreme emotional or physical stress.[3] Treatment depends on the type of cardiomyopathy and the severity of symptoms.[5] Treatments may include lifestyle changes, medications, or surgery.[5] In 2015 cardiomyopathy and myocarditis affected 2.5 million people.[6] Hypertrophic cardiomyopathy affects about 1 in 500 people while dilated cardiomyopathy affects 1 in 2,500.[3][9] They resulted in 354,000 deaths up from 294,000 in 1990.[10][7] Arrhythmogenic right ventricular dysplasia is more common in young people.[2]"

# Extract names of features and annotations in two separate vectors
features <- names(pathologies)
annotations <- unname(unlist(pathologies))

# We'll use dplyr pipe (%>%) to improve readibility of forthcoming code
library(dplyr)

# Load the anclust functions library
# Change the source directory according to your environment
source("R/anclust.r")

# The main anclust() function is used to analyze features and annotations
# Since we don't know yet the optimal number of clusters, we'll let the k parameter to the default value
# We'll use the parameter graph = FALSE to specificy we do not want to compute any figure for now
# See function description for further details
x <- anclust(features, annotations, graph = FALSE)

# The plot() function is used to compute figures from an AnclustObject
# By choosing type="height", we'll plot the hierarchical clustering delta height as a function of the number of clusters
x %>% plot("height")
# Based on the plot output above, we'll choose k=5 as the optimal number of clusters

# We can re-run the analysis of the annotations by specifying k=5 as the fixed number of clusters
x <- anclust(features, annotations, k = 5, graph = FALSE)

# We can print a short description of our AnclustObject by using the print() function
x

# Or a complete summary of the AnclustObject by using the summary() function
x %>% summary()

# By using the plot() function without any specific parameter
# We'll output the dendrogram of the AnclustObject hierarchical clustering
x %>% plot()

# We can use the getClusters() function to list clusters from an AnclustObject, by descending size
x %>% getClusters
# We can use the getFeatures() function to see each submitted feature's assigned cluster
x %>% getFeatures
# We can use the getTerms() function to list all terms retrieved in the annotations of a specific cluster
# With each term's tf (term frequency) and idf (specificity)
x %>% getTerms('A')

# Finally, we can output the word clouds for each cluster
# Based on term frequency (tf)
for(cluster in x %>% getClusters) {
  x %>% cloud(cluster, method = "frequency")
}

# Or tf-idf (term frequency-inverse document frequency)
for(cluster in x %>% getClusters) {
  x %>% cloud(cluster, method = "specificity")
}
```
