Для компіляціх статично було використано makefile у директорії make. Скрипт наведений там  В загальних рисах він компілює все стаично та виводить готову виконавчу програму в main_static

Аби отримати доспут до мапи памяті використав 
./main_static&
vmmap id_ (ми отримуємо одразу айді при ./main_static&<img width="1109" height="646" alt="Screenshot 2025-11-06 at 11 51 46" src="https://github.com/user-attachments/assets/b3fea859-b48d-44ec-b98e-20576274110f" />
<img width="1119" height="80" alt="Screenshot 2025-11-06 at 11 59 53" src="https://github.com/user-attachments/assets/1a193a69-9c1b-4ac0-8946-deda5b042bb2" />


<img width="1347" height="854" alt="Screenshot 2025-11-06 at 11 53 12" src="https://github.com/user-attachments/assets/a1c3d3a0-11c7-4540-b666-e3f21f263fff" />




резульат програми 
luzefik@Andriis-MacBook-Air make % Local vars in function critical_function from lib1: 
        &a = 0x16b68ad64
        &b = 0x16b68ad60
Local vars in function important_function from lib2: 
        &x = 0x16b68ad60
        &y = 0x16b68ad58
Local vars in function main: 
        &t1= 0x16b68ad97
        &t2= 0x16b68ad96
Global variables: 
Main: 
        &main_global_int    = 0x10477c000
        &main_global_double = 0x10477c008
Lib1: 
        &lib1_global_int    = 0x10477c010
        &lib1_global_double = 0x10477c018
Lib2: 
        &lib2_global_int    = 0x10477c020
        &lib2_global_double = 0x10477c028
Function pointers: 
        &main                      = 0x104774580
        &function (lib1) = 0x1047752bc
        &other_function  (lib2) = 0x104775424
Dynamic variables: 
        ptr1 allocated in main: 0x104e11ab0
        ptr2 allocated in main: 0x104e11ac0
        ptr3 allocated in lib1: 0x104e11ad0
        ptr4 allocated in lib2: 0x104e11ae0

Сегмент __TEXT (Код):

vmmap показує діапазон: 104774000-104778000

&main = 0x104774580 (потрапляє в діапазон)

&function (lib1) = 0x1047752bc (потрапляє в діапазон)

&other_function (lib2) = 0x104775424 (потрапляє в діапазон)

код статично злінкованих lib1 та lib2, який vmmap позначає як r-x (read-execute).

Сегмент __DATA (Глобальні змінні):

vmmap показує діапазон: 10477c000-104780000


&main_global_int = 0x10477c000 (точно початок сегмента!)

&lib1_global_int = 0x10477c010 (потрапляє в діапазон)

&lib2_global_int = 0x10477c020 (потрапляє в діапазон)

Це сегмент .data  програми (позначений rw-), де лежать всі ініціалізовані глобальні змінні.

Сегмент Stack (Стек):

vmmap показує діапазон: 16ae90000-16b68c000


&t1= 0x16b68ad97 (потрапляє в діапазон)

&a = 0x16b68ad64 (потрапляє в діапазон)

 Це стек (rw-), де живуть ваші локальні змінні.

Сегмент Heap (Купа):

vmmap показує діапазон: MALLOC_TINY 104e00000-105200000

ptr1 = 0x104e11ab0 (потрапляє в діапазон)

ptr3 = 0x104e11ad0 (потрапляє в діапазон)

Це купа (rw-), звідки виділяється пам'ять через new.




##CMAKELISTS
<img width="1040" height="143" alt="Screenshot 2025-11-06 at 12 04 18" src="https://github.com/user-attachments/assets/dad3a5a9-89d6-47a2-8901-877d9aa0c056" />
<img width="1037" height="286" alt="Screenshot 2025-11-06 at 12 04 06" src="https://github.com/user-attachments/assets/66e07121-79c3-4bd0-9c63-e6216699765a" />
<img width="1008" height="570" alt="Screenshot 2025-11-06 at 12 03 39" src="https://github.com/user-attachments/assets/ba19af5c-73e7-4d4b-a6ff-e5d468232e31" />


рузельтат програми 
luzefik@Andriis-MacBook-Air cmakelist % Local vars in function critical_function from lib1: 
        &a = 0x16b076d1c
        &b = 0x16b076d18
Local vars in function important_function from lib2: 
        &x = 0x16b076d18
        &y = 0x16b076d10
Local vars in function main: 
        &t1= 0x16b076d9f
        &t2= 0x16b076d9e
Global variables: 
Main: 
        &main_global_int    = 0x104d90000
        &main_global_double = 0x104d90008
Lib1: 
        &lib1_global_int    = 0x104db0000
        &lib1_global_double = 0x104db0008
Lib2: 
        &lib2_global_int    = 0x104dc0000
        &lib2_global_double = 0x104dc0008
Function pointers: 
        &main                      = 0x104d89610
        &function (lib1) = 0x104da9110
        &other_function  (lib2) = 0x104db9110
Dynamic variables: 
        ptr1 allocated in main: 0x1053e5b10
        ptr2 allocated in main: 0x1053e5b20
        ptr3 allocated in lib1: 0x1053e5980
        ptr4 allocated in lib2: 0x1053e5990

    ---


Решта сегментів 

Стек: &t1= 0x16b076d9f — все ще знаходиться у "високих" адресах, у регіоні Stack ... [ 8176K ... ] rw-.

Купа: ptr1 = 0x1053e5b10 — знаходиться у регіоні MALLOC_SMALL ... rw-.

