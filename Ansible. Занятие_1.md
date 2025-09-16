1. ansible-playbook -i inventory/test.yml site.yml 
<img width="1243" height="478" alt="Снимок экрана от 2025-09-15 16-42-02" src="https://github.com/user-attachments/assets/379e1ea4-5555-4e17-a000-e55bcc8fe497" />

2. all/example.yml<br>
<img width="459" height="156" alt="Снимок экрана от 2025-09-15 18-38-20" src="https://github.com/user-attachments/assets/1949c9c5-11ed-4802-ad4b-ffc86100913d" />

3. docker ps<br>
<img width="1117" height="106" alt="Снимок экрана от 2025-09-15 18-39-15" src="https://github.com/user-attachments/assets/dbe7bc5e-5d14-49c8-b39f-ee419df0af31" />


4. ansible-playbook -i inventory/prod.yml asite.yml

![Screenshot_20250915_193204_Termux](https://github.com/user-attachments/assets/e476019d-21bf-4983-9842-199c4f2f74e0)

5. 6. ansible-playbook -i inventory/prod.yml site.yml

![Screenshot_20250915_200707_Termux](https://github.com/user-attachments/assets/26818ffb-769d-4103-83c7-1ca94e7199b2)
  
7. ansible-vault encrypt group_vars/el/examp.yml

![Screenshot_20250915_204112_Termux](https://github.com/user-attachments/assets/a4007829-cc80-4caf-a6f3-8f9f34b04173)

8. ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass

![Screenshot_20250916_075105_Termux](https://github.com/user-attachments/assets/27325bf8-322b-4939-9cb0-e2d8470858f6)

9. ansible-doc --list<br>
   ansible-doc community.docker.current_container_facts<br>
   site.yml<br>
   
![Screenshot_20250916_081423_Termux](https://github.com/user-attachments/assets/4215629c-c8ae-4234-90ad-f99d588e6f34)

   ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
   
![Screenshot_20250916_081548_Termux](https://github.com/user-attachments/assets/5e2d0113-d3ac-427b-9dd1-3f0ccb380d8a)

10. nano inventory/prod.yml

![Screenshot_20250916_082453_Termux](https://github.com/user-attachments/assets/d67311ae-c399-4cbc-b050-8299fd3db21f)

11. 
