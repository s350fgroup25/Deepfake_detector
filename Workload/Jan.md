## 4/1/2026
- pitch desk ppt
## 10/1/2026 
- interim report
## 12/1/2026
- downgrade to no-wifi part , also test local min can work or not , and test result

#### Task 1 : Check : 
- using single_evaluate.py

           nano single_evaluate.py
          /home/carmen/asvspoof/datasets/test/record_sample1.wav
          home/carmen/asvspoof/datasets/test/fake1.wav

- Target :

        record_sample1.wav: Positive class probability: 0.1345
        record_sample1.wav: Positive class probability: 0.3442
        fake1:Positive class probability: 0.0006
        fake2:Positive class probability: 0.0144
        fake3:Positive class probability: 0.0424
- JupyterHub :

        record_sample1.wav: Positive class probability: 0.0000
        record_sample1.wav: Positive class probability: 1.0000 (new 0.0000)
        fake1:Positive class probability: 1.0000
        fake2:Positive class probability: 0.0000
        fake3:Positive class probability: 1.0000
=> need to consider the accuracy of the model 

#### Task 2 : local mic 
source venv2_asvspoof/bin/activate
python ~/asvspoof/program/app.py
- http://localhost:5001/record
- but 192.168.1.149 need getUserMedia / HTTPS
- have sound , work 

#### Task 3 : no wifi 
- try to downngrade to 2.4.0 but cant and more error
- now create a new venv -- venv2_asvspoof
- also create a backup :  ~/asvspoof_backup/requirements_working.txt
  
### 26/1/2026
##### Part 1 : 
- test the new model in jupterlab (success)
- download model.py , single.py , model.safetensors
-  <img width="874" height="416" alt="image" src="https://github.com/user-attachments/assets/a0755ba3-e34e-4d2b-94b7-923ad798074b" />
##### Part 2 : test in raspberry pi 
- download
- run 
