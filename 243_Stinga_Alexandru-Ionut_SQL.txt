--Cerinta 4
CREATE SEQUENCE utilizator_seq START WITH 1 INCREMENT BY 1; 
CREATE SEQUENCE galerie_seq START WITH 1 INCREMENT BY 1; 
CREATE SEQUENCE event_seq START WITH 1 INCREMENT BY 1; 
CREATE SEQUENCE exponat_seq START WITH 1 INCREMENT BY 1; 
CREATE SEQUENCE bilet_seq START WITH 1 INCREMENT BY 1; 
CREATE SEQUENCE angajat_seq START WITH 1 INCREMENT BY 1; 
CREATE SEQUENCE orar_seq START WITH 1 INCREMENT BY 1;
CREATE TABLE Utilizator ( 
utilizator_id INT DEFAULT utilizator_seq.NEXTVAL PRIMARY KEY,
nume VARCHAR(50),
prenume VARCHAR(50), 
data_nastere DATE, 
telefon VARCHAR(15), 
email VARCHAR(100) );
CREATE TABLE Galerie ( 
galerie_id INT DEFAULT galerie_seq.NEXTVAL PRIMARY KEY,
 nume VARCHAR(100),
 permanenta VARCHAR(1),
 data_incheiere DATE );
CREATE TABLE Eveniment ( 
event_id INT DEFAULT event_seq.NEXTVAL PRIMARY KEY,
 nume VARCHAR(100),
 data_inceput DATE,
 data_incheiere DATE,
 descriere VARCHAR(300) );
CREATE TABLE Sponsor (
 nume_sponsor VARCHAR(100) PRIMARY KEY,
 adresa_sediu VARCHAR(255),
 fax VARCHAR(20),
 telefon VARCHAR(15),
 nume_CEO VARCHAR(100) );
CREATE TABLE Categorie (
 nume_categorie VARCHAR(100) PRIMARY KEY,
 perioada VARCHAR(50),
 zona VARCHAR(50),
 descriere VARCHAR(300) );
CREATE TABLE Exponat(
 exponat_id INT DEFAULT exponat_seq.NEXTVAL PRIMARY KEY,
 galerie_id INT REFERENCES Galerie(galerie_id),
 nume_categorie VARCHAR(100) REFERENCES Categorie(nume_categorie),
 nume VARCHAR(100),
 an INT,
 creator VARCHAR(100) );
CREATE TABLE Angajat (
 angajat_id INT DEFAULT angajat_seq.NEXTVAL PRIMARY KEY,
 galerie_id INT REFERENCES Galerie(galerie_id),
 tip VARCHAR(50),
 vechime INT,
 nume VARCHAR(100),
 varsta INT,
 telefon VARCHAR(15),
 email VARCHAR(100));
CREATE TABLE Ghid (
 angajat_id INT PRIMARY KEY,
 FOREIGN KEY(angajat_id) REFERENCES Angajat(angajat_id),
 limba VARCHAR(50),
salariu_ghid NUMBER(10, 2));
CREATE TABLE Supervizor (
 angajat_id INT PRIMARY KEY,
 FOREIGN KEY(angajat_id) REFERENCES Angajat(angajat_id),
 pozitie VARCHAR(100),
 obligatii VARCHAR(4000),
salariu_supervizor NUMBER(10, 2));
CREATE TABLE Orar (
 orar_id INT DEFAULT orar_seq.NEXTVAL PRIMARY KEY,
 angajat_id INT REFERENCES Angajat(angajat_id),
 galerie_id INT REFERENCES Galerie(galerie_id),
 data_orar DATE );
CREATE TABLE Bilet (
 bilet_id INT DEFAULT bilet_seq.NEXTVAL PRIMARY KEY,
 utilizator_id INT REFERENCES Utilizator(utilizator_id),
 tip VARCHAR(50),
 orar_id INT REFERENCES Orar(orar_id));
CREATE TABLE Bilet_Galerie (
 bilet_id INT PRIMARY KEY ,
FOREIGN KEY(bilet_id) REFERENCES Bilet(bilet_id),
 galerie_id INT REFERENCES Galerie(galerie_id),
pret_galerie NUMBER(10, 2) );
CREATE TABLE Bilet_Eveniment (
 bilet_id INT  PRIMARY KEY ,
FOREIGN KEY(bilet_id) REFERENCES Bilet(bilet_id),
 event_id INT REFERENCES Eveniment(event_id),
pret_eveniment NUMBER(10, 2) );
CREATE TABLE Supervizat_Supervizor (
 angajat_id_supervizat INT,
 angajat_id_supervizor INT,
 PRIMARY KEY(angajat_id_supervizat,angajat_id_supervizor),
 FOREIGN KEY(angajat_id_supervizat) REFERENCES Angajat (angajat_id),
 FOREIGN KEY(angajat_id_supervizor) REFERENCES Angajat(angajat_id));
 CREATE TABLE Galerie_Eveniment (
 galerie_id INT,
 event_id INT,
 PRIMARY KEY(galerie_id,event_id),
 FOREIGN KEY(galerie_id) REFERENCES Galerie (galerie_id),
 FOREIGN KEY(event_id) REFERENCES Eveniment(event_id));
 CREATE TABLE Eveniment_Sponsor (
 event_id INT,
 nume_sponsor VARCHAR(100),
 PRIMARY KEY(event_id,nume_sponsor),
 FOREIGN KEY(event_id) REFERENCES Eveniment (event_id),
 FOREIGN KEY(nume_sponsor) REFERENCES Sponsor(nume_sponsor));

--Cerinta 5
INSERT INTO Utilizator (nume, prenume, data_nastere, telefon, email) VALUES
('Popescu', 'Ion', TO_DATE('1985-05-15', 'YYYY-MM-DD'), '0741234567', 'popion@yahoo.com');
INSERT INTO Utilizator (nume, prenume, data_nastere, telefon, email) VALUES
('Ionescu', 'Maria', TO_DATE('1990-07-20', 'YYYY-MM-DD'), '0742345678', 'maria9ionescu@gmail.com');
INSERT INTO Utilizator (nume, prenume, data_nastere, telefon, email) VALUES
('Georgescu', 'Alexandru', TO_DATE('1978-03-10', 'YYYY-MM-DD'), '0743456789', 'alexandru_georgescu@yahoo.com');
INSERT INTO Utilizator (nume, prenume, data_nastere, telefon, email) VALUES
('Vasilescu', 'Elena', TO_DATE('1982-11-25', 'YYYY-MM-DD'), '0744567890', 'elena.vasilescu@gmail.com');
INSERT INTO Utilizator (nume, prenume, data_nastere, telefon, email) VALUES
('Marin', 'Daniel', TO_DATE('2000-01-30', 'YYYY-MM-DD'), '0745678901', 'daniel00marin@yahoo.com');
INSERT INTO Sponsor (nume_sponsor, adresa_sediu, fax, telefon, nume_CEO) VALUES
('Dacia', 'Calea Floreasca 166, Bucuresti', '0211234567', '0721123456', 'Christophe Dridi');
INSERT INTO Sponsor (nume_sponsor, adresa_sediu, fax, telefon, nume_CEO) VALUES
('BCR', 'Bd. Regina Elisabeta 5, Bucuresti', '0212345678', '0722234567', 'Sergiu Manea');
INSERT INTO Sponsor (nume_sponsor, adresa_sediu, fax, telefon, nume_CEO) VALUES
('Petrom', 'Str. Coralilor 22, Bucuresti', '0213456789', '0723345678', 'Christina Verchere');
INSERT INTO Sponsor (nume_sponsor, adresa_sediu, fax, telefon, nume_CEO) VALUES
('Bitdefender', 'Str. Barbu Vacarescu 54, Bucuresti', '0214567890', '0724456789', 'Florin Talpes');
INSERT INTO Sponsor (nume_sponsor, adresa_sediu, fax, telefon, nume_CEO) VALUES
('eMAG', 'Str. Bdul. Pierre de Coubertin 3-5, Bucuresti', '0215678901', '0725567890', 'Iulian Stanciu');
INSERT INTO Exponat (galerie_id, nume_categorie, nume, an, creator) VALUES
(1, 'Pictura sec XIX', 'Ciobanas cu turma de oi', 1895, 'Nicolae Grigorescu');

INSERT INTO Exponat (galerie_id, nume_categorie, nume, an, creator) VALUES
(2, 'Sculptura sec XIX', 'Domnitorul Alexandru Ioan Cuza', 1860, 'Ion Georgescu');
INSERT INTO Exponat (galerie_id, nume_categorie, nume, an, creator) VALUES
(3, 'Arta Populara sec XX', 'Covor Oltenesc', 1920, 'Artisan Popular');
INSERT INTO Exponat (galerie_id, nume_categorie, nume, an, creator) VALUES
(4, 'Fotografie sec XX', 'Revolutia din 1989', 1989, 'Fotoreporter Necunoscut');
INSERT INTO Exponat (galerie_id, nume_categorie, nume, an, creator) VALUES
(5, 'Arta Contemporana', 'Ingerul', 2005, 'Ion Barladeanu');
INSERT INTO EXPONAT (GALERIE_ID, NUME_CATEGORIE, NUME, AN, CREATOR) VALUES ('21', 'Pictura Moderna', 'Clar de luna', '1990', 'IOn Tuculescu');
INSERT INTO EXPONAT (GALERIE_ID, NUME_CATEGORIE, NUME, AN, CREATOR) VALUES ('2', 'Sculptura sec XIX', 'Bust Stefan cel Mare', '1850', 'IOn Georgescu');
INSERT INTO Galerie (nume, permanenta, data_incheiere) VALUES
('Galeria de Arta Moderna', 'Y', NULL);
INSERT INTO Galerie (nume,permanenta, data_incheiere) VALUES
('Galeria de Istorie Antica', 'N', TO_DATE('2023-12-31', 'YYYY-MM-DD'));
INSERT INTO Galerie (nume, permanenta, data_incheiere) VALUES
('Galeria de Stiinta', 'Y', NULL);
INSERT INTO Galerie (nume, permanenta, data_incheiere) VALUES
('Galeria de Arta Contemporana', 'N', TO_DATE('2024-06-30', 'YYYY-MM-DD'));
INSERT INTO Galerie (nume, permanenta, data_incheiere) VALUES
('Galeria de Fotografie', 'Y', NULL);
INSERT INTO GALERIE (NUME, PERMANENTA) VALUES ('Galeria Art Safari', 'Y');
INSERT INTO Categorie (nume_categorie, perioada, zona, descriere) VALUES
('Pictura sec XIX', '1800-1900', 'Europa de Est', 'Picturi din secolul XIX din Europa de Est');
INSERT INTO Categorie (nume_categorie, perioada, zona, descriere) VALUES
('Sculptura sec XIX', '1800-1900', 'Europa de Est', 'Sculpturi din secolul XIX din Europa de Est');


INSERT INTO Categorie (nume_categorie, perioada, zona, descriere) VALUES
('Arta Populara sec XX', '1900-2000', 'Romania', 'Obiecte de arta populara romaneasca din secolul XX');
INSERT INTO Categorie (nume_categorie, perioada, zona, descriere) VALUES
('Fotografie sec XX', '1900-2000', 'Romania', 'Fotografii istorice din Romania secolului XX');
INSERT INTO Categorie (nume_categorie, perioada, zona, descriere) VALUES
('Arta Contemporana', '2000-prezent', 'Romania', 'Lucrari de arta contemporana din Romania');
INSERT INTO CATEGORIE (NUME_CATEGORIE, PERIOADA, ZONA, DESCRIERE) VALUES ('Pictura Moderna', '1900-2000', 'Romania', 'O expozitie de lucrari din perioada Moderna a Romaniei');
INSERT INTO CATEGORIE (NUME_CATEGORIE, PERIOADA, ZONA, DESCRIERE) VALUES ('Murale din Oceania', 'Necunoscuta', 'Oceania', 'O colectie de lucrari murale aduse din Oceania ');
INSERT INTO Eveniment (nume, data_inceput, data_incheiere, descriere) VALUES
('Noaptea Muzeelor', TO_DATE('2024-05-18', 'YYYY-MM-DD'), TO_DATE('2024-05-18', 'YYYY-MM-DD'), 'Eveniment special cu acces gratuit.');
INSERT INTO Eveniment (nume, data_inceput, data_incheiere, descriere) VALUES
('Expozitia de Vara', TO_DATE('2024-06-01', 'YYYY-MM-DD'), TO_DATE('2024-08-31', 'YYYY-MM-DD'), 'Expozitie temporara de vara.');
INSERT INTO Eveniment (nume, data_inceput, data_incheiere, descriere) VALUES
('Festivalul de Arta', TO_DATE('2024-09-15', 'YYYY-MM-DD'), TO_DATE('2024-09-30', 'YYYY-MM-DD'), 'Festival de arta contemporana.');
INSERT INTO Eveniment (nume, data_inceput, data_incheiere, descriere) VALUES
('Saptamana Istoriei', TO_DATE('2024-10-01', 'YYYY-MM-DD'), TO_DATE('2024-10-07', 'YYYY-MM-DD'), 'Saptamana dedicata istoriei.');
INSERT INTO Eveniment (nume, data_inceput, data_incheiere, descriere) VALUES
('Zilele Culorii', TO_DATE('2024-11-10', 'YYYY-MM-DD'), TO_DATE('2024-11-20', 'YYYY-MM-DD'), 'Eveniment dedicat oricarui tip de pata de culoare.');
INSERT INTO Angajat (galerie_id, tip, vechime, nume, varsta, telefon, email) VALUES (1, 'ghid', 3, 'Ivan Ion', 32, '0741122334', 'ion_ivan13@yahoo.com');
INSERT INTO Angajat (galerie_id, tip, vechime, nume, varsta, telefon, email) VALUES (3, 'ghid', 5, 'Vladimirescu Cornel', 28, '0755566778', 'corneliusvlad@gmail.com');
INSERT INTO Angajat (galerie_id, tip, vechime, nume, varsta, telefon, email) VALUES (1, 'ghid', 2, 'Grigorescu Ionel', 40, '0766677889', 'ionel-grigorita@yahoo.com');
INSERT INTO Angajat (galerie_id, tip, vechime, nume, varsta, telefon, email) VALUES (1, 'ghid', 4, 'Negoita Florin', 35, '0777788990', 'florinita@gmail.com');
INSERT INTO Angajat (galerie_id, tip, vechime, nume, varsta, telefon, email) VALUES (3, 'ghid', 1, 'Dragulici Marin', 26, '0788899001', 'dragurin@yahoo.com');
INSERT INTO Angajat (galerie_id, tip, vechime, nume, varsta, telefon, email) VALUES (1, 'supervizor', 6, 'Popa Vlad', 45, '0741000000', 'vladivici.popa@yahoo.com');
INSERT INTO Angajat (galerie_id, tip, vechime, nume, varsta, telefon, email) VALUES (1, 'supervizor', 8, 'Veira Radu', 50, '0755000000', 'veiradu@gmail.com');
INSERT INTO Angajat (galerie_id, tip, vechime, nume, varsta, telefon, email) VALUES (3, 'supervizor', 3, 'Istratie Ionut', 35, '0766000000', 'ionut.istratie@yahoo.com');
INSERT INTO Angajat (galerie_id, tip, vechime, nume, varsta, telefon, email) VALUES (3, 'supervizor', 7, 'Stanciu Mario', 48, '0777000000', 'ggmariom8stanciu@gmail.com');
INSERT INTO Angajat (galerie_id, tip, vechime, nume, varsta, telefon, email) VALUES (1, 'supervizor', 2, 'Andreea Dumitrescu', 30, '0788000000', 'andreea.dumitrescu@yahoo.com');
INSERT INTO ANGAJAT (GALERIE_ID, TIP, VECHIME, NUME, VARSTA, TELEFON, EMAIL) VALUES ('2', 'supervizor', '10', 'Ivan Ion', '50', '0753254325', 'ivanovick09@gmail.com');
INSERT INTO Ghid (angajat_id, limba, salariu_ghid) VALUES (5, 'Engleză', 3500.50);
INSERT INTO Ghid (angajat_id, limba, salariu_ghid) VALUES (6, 'Germană', 3700.75);
INSERT INTO Ghid (angajat_id, limba, salariu_ghid) VALUES (7, 'Franceză', 3400.00);
INSERT INTO Ghid (angajat_id, limba, salariu_ghid) VALUES (8, 'Spaniolă', 3600.20);
INSERT INTO Ghid (angajat_id, limba, salariu_ghid) VALUES (9, 'Italiană', 3550.80);
INSERT INTO SUPERVIZOR (ANGAJAT_ID, POZITIE, OBLIGATII, SALARIU_SUPERVIZOR) VALUES ('21', 'Curator', 'Planificarea și organizarea expozițiilor, cercetarea și selectarea operelor de artă, gestionarea colecțiilor', '5800');
INSERT INTO Supervizor (angajat_id, pozitie, obligatii, salariu_supervizor) 
VALUES (10, 'Supervizor de galerie', 'Coordonarea personalului, planificarea și organizarea evenimentelor și expozițiilor, gestionarea operațiunilor zilnice ale galeriei', 4500.00);
INSERT INTO Supervizor (angajat_id, pozitie, obligatii, salariu_supervizor) 
VALUES (11, 'Curator', 'Planificarea și organizarea expozițiilor, cercetarea și selectarea operelor de artă, gestionarea colecțiilor', 4700.00);
INSERT INTO Supervizor (angajat_id, pozitie, obligatii, salariu_supervizor) 
VALUES (12, 'Manager de muzeu', 'Supervizarea întregii activități a muzeului, elaborarea strategiilor de dezvoltare, gestionarea resurselor umane și financiare', 5000.00);
INSERT INTO Supervizor (angajat_id, pozitie, obligatii, salariu_supervizor) 
VALUES (13, 'Conservator', 'Restaurarea și conservarea operelor de artă, elaborarea planurilor de conservare și restaurare, documentarea și cercetarea obiectelor de artă', 4800.00);
INSERT INTO Supervizor (angajat_id, pozitie, obligatii, salariu_supervizor) 
VALUES (14, 'Manager Human Resources', 'Organizarea personalului si rezolvarea oricaror conflicte interne ce pot aparea de-a lungul timpului.', 4600.00);
INSERT INTO Orar (angajat_id, galerie_id, data_orar) VALUES (5, 1, TO_DATE('2024-05-18', 'YYYY-MM-DD'));
INSERT INTO Orar (angajat_id, galerie_id, data_orar) VALUES (6, 3, TO_DATE('2024-05-18', 'YYYY-MM-DD'));
INSERT INTO Orar (angajat_id, galerie_id, data_orar) VALUES (7, 1, TO_DATE('2024-05-18', 'YYYY-MM-DD'));
INSERT INTO Orar (angajat_id, galerie_id, data_orar) VALUES (8, 1, TO_DATE('2024-05-18', 'YYYY-MM-DD'));
INSERT INTO Orar (angajat_id, galerie_id, data_orar) VALUES (9, 3, TO_DATE('2024-05-18', 'YYYY-MM-DD'));
INSERT INTO Bilet (utilizator_id, tip, orar_id) VALUES (1, 'galerie', 1);  
INSERT INTO Bilet (utilizator_id, tip, orar_id) VALUES (1, 'galerie', 2);  
INSERT INTO Bilet (utilizator_id, tip, orar_id) VALUES (4, 'galerie', 3);  
INSERT INTO Bilet (utilizator_id, tip, orar_id) VALUES (3, 'galerie', 4); 
INSERT INTO Bilet (utilizator_id, tip, orar_id) VALUES (4, 'galerie', 5);  
INSERT INTO Bilet (utilizator_id, tip, orar_id) VALUES (2, 'eveniment', 1);
INSERT INTO Bilet (utilizator_id, tip, orar_id) VALUES (2, 'eveniment', 2); 
INSERT INTO Bilet (utilizator_id, tip, orar_id) VALUES (3, 'eveniment', 3); 
INSERT INTO Bilet (utilizator_id, tip, orar_id) VALUES (5, 'eveniment', 4);
INSERT INTO Bilet (utilizator_id, tip, orar_id) VALUES (5, 'eveniment', 5);
INSERT INTO Bilet_Galerie (bilet_id, galerie_id, pret_galerie) VALUES (1, 1, 50.00);
INSERT INTO Bilet_Galerie (bilet_id, galerie_id, pret_galerie) VALUES (2, 3, 60.00);
INSERT INTO Bilet_Galerie (bilet_id, galerie_id, pret_galerie) VALUES (3, 1, 50.00);
INSERT INTO Bilet_Galerie (bilet_id, galerie_id, pret_galerie) VALUES (4, 1, 50.00);
INSERT INTO Bilet_Galerie (bilet_id, galerie_id, pret_galerie) VALUES (5, 3, 60.00);
INSERT INTO Bilet_Eveniment (bilet_id, event_id, pret_eveniment) VALUES (6, 1, 0.00);
INSERT INTO Bilet_Eveniment (bilet_id, event_id, pret_eveniment) VALUES (7, 1, 0.00);
INSERT INTO Bilet_Eveniment (bilet_id, event_id, pret_eveniment) VALUES (8, 5, 75.00);
INSERT INTO Bilet_Eveniment (bilet_id, event_id, pret_eveniment) VALUES (9, 3, 80.00);
INSERT INTO Bilet_Eveniment (bilet_id, event_id, pret_eveniment) VALUES (10, 4, 70.00);
INSERT INTO Supervizat_Supervizor (angajat_id_supervizat, angajat_id_supervizor) VALUES (5, 10);
INSERT INTO Supervizat_Supervizor (angajat_id_supervizat, angajat_id_supervizor) VALUES (5, 11);
INSERT INTO Supervizat_Supervizor (angajat_id_supervizat, angajat_id_supervizor) VALUES (5, 14);
INSERT INTO Supervizat_Supervizor (angajat_id_supervizat, angajat_id_supervizor) VALUES (7, 10);
INSERT INTO Supervizat_Supervizor (angajat_id_supervizat, angajat_id_supervizor) VALUES (7, 11);
INSERT INTO Supervizat_Supervizor (angajat_id_supervizat, angajat_id_supervizor) VALUES (7, 14);
INSERT INTO Supervizat_Supervizor (angajat_id_supervizat, angajat_id_supervizor) VALUES (8, 10);
INSERT INTO Supervizat_Supervizor (angajat_id_supervizat, angajat_id_supervizor) VALUES (8, 11);
INSERT INTO Supervizat_Supervizor (angajat_id_supervizat, angajat_id_supervizor) VALUES (8, 14);
INSERT INTO Supervizat_Supervizor (angajat_id_supervizat, angajat_id_supervizor) VALUES (6, 12);
INSERT INTO Galerie_Eveniment (galerie_id, event_id) VALUES (1, 1);
INSERT INTO Galerie_Eveniment (galerie_id, event_id) VALUES (3, 1);
INSERT INTO Galerie_Eveniment (galerie_id, event_id) VALUES (1, 5);
INSERT INTO Galerie_Eveniment (galerie_id, event_id) VALUES (1, 3);
INSERT INTO Galerie_Eveniment (galerie_id, event_id) VALUES (3, 4);
INSERT INTO Galerie_Eveniment (galerie_id, event_id) VALUES (2, 1);
INSERT INTO Galerie_Eveniment (galerie_id, event_id) VALUES (4, 1);
INSERT INTO Galerie_Eveniment (galerie_id, event_id) VALUES (5, 1);
INSERT INTO Galerie_Eveniment (galerie_id, event_id) VALUES (2, 2);
INSERT INTO Galerie_Eveniment (galerie_id, event_id) VALUES (5, 4);
INSERT INTO GALERIE_EVENIMENT (GALERIE_ID, EVENT_ID) VALUES ('21', '2');
INSERT INTO Eveniment_Sponsor (event_id, nume_sponsor) VALUES (1, 'BCR');
INSERT INTO Eveniment_Sponsor (event_id, nume_sponsor) VALUES (2, 'Petrom');
INSERT INTO Eveniment_Sponsor (event_id, nume_sponsor) VALUES (3, 'Bitdefender');
INSERT INTO Eveniment_Sponsor (event_id, nume_sponsor) VALUES (4, 'eMAG');
INSERT INTO Eveniment_Sponsor (event_id, nume_sponsor) VALUES (5, 'Dacia');
INSERT INTO Eveniment_Sponsor (event_id, nume_sponsor) VALUES (1, 'eMAG');
INSERT INTO Eveniment_Sponsor (event_id, nume_sponsor) VALUES (2, 'BCR');
INSERT INTO Eveniment_Sponsor (event_id, nume_sponsor) VALUES (3, 'eMAG');
INSERT INTO Eveniment_Sponsor (event_id, nume_sponsor) VALUES (4, 'Dacia');
INSERT INTO Eveniment_Sponsor (event_id, nume_sponsor) VALUES (5, 'Bitdefender');

--Cerinta 6
create or replace function Profit (categ in categorie.nume_categorie%type) return number as
    type IndexedCollection is table of galerie.galerie_id%type index by pls_integer;
    type NestedCollection is table of eveniment.event_id%type;
    type Vector is varray(100) of bilet_galerie.pret_galerie%type;
    galerii IndexedCollection;
    evenimente NestedCollection := NestedCollection();
    preturi Vector := Vector();
    profitTotal number := 0;
    exista number;
    ceva categorie.zona%type;
begin
    --Verificarea existentei respectivei categorii
    select zona into ceva from categorie where upper(nume_categorie)=upper(categ);
    --Obtinerea listei galeriilor ce contin exponate cu categoria data ca parametru 
    select distinct galerie_id bulk collect into galerii from exponat where upper(nume_categorie) = upper(categ);
    if galerii.count = 0 then
        raise_application_error(-20002,'Nu exista galerie ce sa aiba o categorie cu acel nume');
    end if;
    --Pentru lista de galerii se obtine in colectii:
    for i in 1..galerii.count loop
    --Lista de evenimente ce contin galeriile in care exista exponate din respectiva categorie
        for r in (select distinct event_id from galerie_eveniment where galerie_id = galerii(i)) loop
            evenimente.extend;
            evenimente(evenimente.count) := r.event_id;
        end loop;
        --Lista cu preturile biletelor pentru galeriile din acea lista
        for r in (select pret_galerie from bilet_galerie where galerie_id = galerii(i)) loop
            if preturi.count < preturi.limit then
                preturi.extend;
                preturi(preturi.count) := r.pret_galerie;
            else
                raise_application_error(-20001, 'Nu se respecta limita suoerioara a vectorului preturi');
            end if;
        end loop;
    end loop;
    --Se adauga si biletele pentru evenimentele obtinute mai devreme
    for i in 1..evenimente.count loop
        for r in (select pret_eveniment from bilet_eveniment where event_id = evenimente(i)) loop
            if preturi.count < preturi.limit then
                preturi.extend;
                preturi(preturi.count) := r.pret_eveniment;
            else
                raise_application_error(-20001, 'Nu se respecta limita suoerioara a vectorului preturi');
            end if;
        end loop;
    end loop;
    --Se calculeaza profitul total
    for i in 1..preturi.count loop
        profitTotal := profitTotal + preturi(i);
    end loop;

    return profitTotal;
exception
    when no_data_found then
        raise_application_error(-20003,'Nu exista o astfel de categorie');
end;
/
--Bloc testare
declare
    categ categorie.nume_categorie%type:='&p_nume';
    galerie_inexistenta exception;
    limita_depasita exception;
    no_categorie exception;
    pragma exception_init(no_categorie,-20003);
    pragma exception_init(galerie_inexistenta,-20002);
    pragma exception_init(limita_depasita,-20001);
begin
    
    dbms_output.put_line(profit('categ'));
exception
    when galerie_inexistenta then
        dbms_output.put_line('Nu exista vreun exponat din categoria primita ca parametru.');
    when limita_depasita then
        dbms_output.put_line('Exista prea multe bilete penntru galerii sau evenimente in care exista exponate din categoria primita ca parametru');
    when no_categorie then
        dbms_output.put_line('Nu exista categoria data ca parametru');
end;
/
--Cerinta 7
create or replace function ghidProfit(numele in angajat.nume%type) return number as
    -- Cursor parametrizat pentru a extrage preturile biletelor asociate unui orar specific.
    cursor obtPretGal(idul orar.orar_id%type) is
        select bg.pret_galerie from bilet_galerie bg
                            join bilet b on b.bilet_id=bg.bilet_id where b.orar_id=idul;
    idul orar.orar_id%type;
    profit number(4):=0;
    idAngajat angajat.angajat_id%type; 
    tipul angajat.tip%type;
begin
    -- Preia ID-ul si tipul angajatului pe baza numelui dat ca parametru.
    select angajat_id, nvl(tip,'') into idAngajat,tipul from angajat
        where trim(upper(nume))=trim((upper(numele)));
    -- Verifica daca angajatul este de tip "GHID"; altfel, arunca o exceptie.
    if trim(upper(tipul))<>'GHID' then
        raise_application_error(-20001,'Numele primit ca parametru nu apartine unui Ghid');
    end if;
    -- Itereaza prin toate orarele asociate angajatului dat.
    for orar_rec in (select o.orar_id from orar o
                    join angajat a on a.angajat_id=o.angajat_id
                    where a.nume=numele) loop
        idul:=orar_rec.orar_id; 
        -- Aduna profitul din biletele de galerie asociate acestui orar.
        for pret_rec in obtPretGal(idul) loop
            profit:=profit+pret_rec.pret_galerie;
        end loop;
        -- Aduna profitul din biletele de eveniment asociate acestui orar.
        for pret_rec in (select be.pret_eveniment from bilet_eveniment be
                            join bilet b on b.bilet_id=be.bilet_id where b.orar_id=idul) loop
            profit:=profit+pret_rec.pret_eveniment;
        end loop;
    end loop;
    return profit;
exception
    when no_data_found then
        raise_application_error(-20000,'Nu exista angajat cu acest nume');
    when too_many_rows then
        raise_application_error(-20002,'Exista mai multi angajati cu acest nume');
end;
/
--Bloc testare
declare 
    nume angajat.nume%type:='&p_nume';
    no_Angajat exception;
    not_Ghid exception;
    many_Angajati exception;
    pragma exception_init(no_Angajat,-20000);
    pragma exception_init(not_Ghid,-20001);
    pragma exception_init(many_Angajati,-20002);
begin
    dbms_output.put_line('Profitul adus de ghidul '||nume||' este de '||ghidProfit(nume));
exception
    when no_Angajat then 
        dbms_output.put_line('Nu exista vreun angajat cu numele '||nume);
    when not_Ghid then
        dbms_output.put_line('Angajatul '||nume||' nu este un ghid.');
    when many_Angajati then
        dbms_output.put_line('Exista mai multi angajati cu numele '||nume);
    when others then
        dbms_output.put_line('Eroare necunoscuta.');
end;
/
--Cerinta 8
create or replace function OrareSiAngajati(numele in galerie.nume%type) return varchar2 as
    type galerieRecord is record (nume galerie.nume%type,nr_orare number,nr_angajati number);
    returnare galerieRecord;
    idul galerie.galerie_id%type;
begin
    select g.galerie_id into idul from galerie g where upper(trim(numele))=upper(trim(g.nume));
    select g.nume, count(o.orar_id) as nr_orare, 
           (select count(*) from angajat a where a.galerie_id=g.galerie_id) as nr_angajati
    into returnare from galerie g
    left join orar o on o.galerie_id=g.galerie_id
    group by g.nume, g.galerie_id
    having (select count(*) from angajat a where a.galerie_id=g.galerie_id)>count(o.orar_id)
       and (select count(*) from angajat a where a.galerie_id = g.galerie_id) - count(o.orar_id) = 
            (select max((select count(*) from angajat a where a.galerie_id = gi.galerie_id) - count(oi.orar_id))
             from galerie gi
             left join orar oi on oi.galerie_id = gi.galerie_id
             group by gi.galerie_id);
    if upper(trim(numele))=upper(trim(returnare.nume)) then
        return 'DA';
    else
        return 'NU';
    end if;
exception
    when no_data_found then
        raise_application_error(-20000,'Nu exista galerie cu un astfel de nume');
    when too_many_rows then
        raise_application_error(-20001,'Exista mai multe galerii ce au diferenta dintre nr de angajati si orare maxima');
    when others then
        raise_application_error(-20002,'Eroare neasteptata');
end;
/
--Bloc testare
declare 
    nume galerie.nume%type:='&p_nume';
    no_Galerie exception;
    diverse exception;
    many_Galerii exception;
    pragma exception_init(no_Galerie,-20000);
    pragma exception_init(diverse,-20002);
    pragma exception_init(many_Galerii,-20001);
begin
    dbms_output.put_line(OrareSiAngajati(nume));
exception
    when no_Galerie then 
        dbms_output.put_line('Nu exista vreo galerie denumita '||nume);
    when diverse then
        dbms_output.put_line('Eroare necunoscuta');
    when many_Galerii then
        dbms_output.put_line('Exista mai multe galerii cu difernta maxima intre angajati si orare');
end;
/
--Cerinta 9
create or replace procedure Galerie_Detalii ( numele in galerie.nume%type, data_s in eveniment.data_inceput%type, data_f in eveniment.data_incheiere%type) as
    -- Declararea variabilelor pentru stocarea numarului de evenimente si ID-ul galeriei
    nr_evenimente number;
    id_galerie galerie.galerie_id%type;
    -- Declararea exceptiilor personalizate
    no_Events exception;
    invalid_interval exception;
    -- Asocierea codurilor de eroare pentru exceptii
    pragma exception_init(no_Events,-20002);
    pragma exception_init(invalid_interval,-20003);
begin
    -- Cautarea ID-ului galeriei dupa nume; verificare insensibila la spatii si litere mari/mici
    select g.galerie_id into id_galerie from galerie g where upper(trim(g.nume))=upper(trim(numele));
    -- Verificarea daca intervalul de date este valid
    if data_s>data_f then
        raise invalid_interval;
    end if;
    -- Determinarea numarului de evenimente in intervalul specificat
    select count(*) into nr_evenimente from galerie_eveniment ge
        join eveniment e on e.event_id=ge.event_id
        where ge.galerie_id=id_galerie and ((e.data_inceput between data_s and data_f) or (e.data_incheiere between data_s and data_f));
    -- Verificarea daca exista evenimente in acest interval; daca nu, se ridica o exceptie
    if nr_evenimente=0 then
        raise_application_error(-20002,'Nu exista evenimente in acest interval.');
    end if;
    -- Iterarea prin evenimentele gasite si afisarea detaliilor
    for r in (select e.nume, e.data_inceput, e.data_incheiere, s.nume_sponsor,s.nume_ceo,count(be.bilet_id) as nr_bilete from galerie_eveniment ge
        join eveniment e on e.event_id=ge.event_id
        left join eveniment_sponsor es on es.event_id=e.event_id
        left join sponsor s on s.nume_sponsor=es.nume_sponsor
        left join bilet_eveniment be on be.event_id=e.event_id
        where ge.galerie_id=id_galerie and ((e.data_inceput between data_s and data_f) or (e.data_incheiere between data_s and data_f))
        group by e.nume, e.data_inceput, e.data_incheiere, s.nume_sponsor, s.nume_ceo) loop
            -- Afisarea informatiilor despre fiecare eveniment
            dbms_output.put_line('Eveniment: ' || r.nume || 
                                 ', Perioada: ' || r.data_inceput || ' - ' || r.data_incheiere ||
                                 ', Sponsor: ' ||nvl(r.nume_sponsor, 'Nespecificat') || 
                                 ', CEO Sponsor: ' || nvl(r.nume_ceo, 'Nespecificat') ||
                                 ', Bilete vandute: ' || r.nr_bilete);
    end loop;
exception
    when no_data_found then
        dbms_output.put_line('Nu exista galerie cu numele '||numele);
    when too_many_rows then
         dbms_output.put_line('Exista mai multe galerii cu numele '||numele);
    when no_events then
        dbms_output.put_line('Galeria '||numele||' nu este inclusa in niciun eveniment.');
    when invalid_interval then
        dbms_output.put_line('Perioada oferita ca parametru nu este valida. Al doilea parametru este data de inceput, iar al treilea este data de sfarsit.');
    when others then
        dbms_output.put_line('Eroare neasteptata!');
end;
/
--Bloc testare exemplu
begin
    Galerie_Detalii('Galeria de Arta Moderna',to_date ('01-MAY-24','DD-MON-YY'),to_Date ('12-NOV-24','DD-MON-YY'));
end;
/
--Cerinta 10
create or replace trigger Restrictie_Exponate before insert on exponat 
declare
nr_exponate number;
begin
    select count(*) into nr_exponate from exponat;
    if nr_exponate>10 then
        raise_application_error(-20000,'S-a trecut peste limita totala de 10 exponate pe care galeriile le pot detine.');
    end if;
end;
/
--Bloc testare
INSERT INTO Exponat (galerie_id, nume_categorie, nume, an, creator) VALUES
(1, 'Pictura sec XIX', 'Cioban cu multe oi', 1905, 'Nicolae Grigorescu');

INSERT INTO Exponat (galerie_id, nume_categorie, nume, an, creator) VALUES
(2, 'Sculptura sec XIX', 'Domnitorul Vlad Tepes', 1870, 'Ion Georgescu');

INSERT INTO Exponat (galerie_id, nume_categorie, nume, an, creator) VALUES
(3, 'Arta Populara sec XX', 'Covor Banatean', 1925, 'Artisan Popular');

INSERT INTO Exponat (galerie_id, nume_categorie, nume, an, creator) VALUES
(4, 'Fotografie sec XX', 'Doborarea Zidului Berlinului', 1983, 'Fotoreporter Necunoscut');
/
--Cerinta 11
create or replace trigger Verificare_Galerie before insert on Galerie for each row
declare
    nr_galerii number;
begin
    select count(*) into nr_galerii from galerie g
        where upper(trim(g.nume))=upper(trim(:new.nume));
    if nr_galerii>0 then
        raise_application_error(-20000,'Numele galeriilor trebuie sa fie unic.');
    end if;
    if upper(trim(:new.permanenta))='Y' then
        if :new.data_incheiere is not null then
            raise_application_error(-20001,'Galeria este trecuta ca permanenta, insa a fost adaugata o data de incheiere.');
        end if;
    elsif upper(trim(:new.permanenta))='N' then
        if :new.data_incheiere is null then
            raise_application_error(-20002,'Galeria este trecuta ca temporala, insa nu exista vreo data de incheiere.');
        end if;
    else
        raise_application_error(-20003,'Atributul permanenta poate sa aiba doar doua valori, Y daca este permanenta si N daca este temporala.');
    end if;
end;
/
--Bloc testare exemplu
INSERT INTO Galerie (nume, permanenta, data_incheiere)
VALUES ('Galeria de Arta', 'Y', TO_DATE('2024-09-24','YYYY-MM-DD'));
INSERT INTO Galerie (nume, permanenta)
VALUES ('Galeria de Arta', 'N');
/
--Cerinta 12
create table amprenta_user (
    nume_bd varchar2(50),
    nume_utilizator varchar2(30),
    eveniment varchar2(40),
    tipul_obiectului varchar2(40),
    numele_obiectului varchar2(40),
    data timestamp(3),
    stare varchar2(10) check( stare in ('Acceptata', 'Anulata')));

create or replace trigger AuditSiOre_de_Lucru before create or alter or drop on schema
declare
    ora number;
    pragma autonomous_transaction; 
--Am utilizat aceasta instructiune deoarece fara ea raise_application_error intrerupea orice tranzactie, inclusive cele de tip audit pe care voiam sa le introduce in tabela Amprenta_User.
begin
    ora:= to_number(to_char(sysdate,'HH24'));
   insert into amprenta_user
        values (
            sys.database_name, 
            sys.login_user, 
            sys.sysevent,
            sys.dictionary_obj_type,
            sys.dictionary_obj_name,
            current_timestamp,
            case 
                when ora not between 8 and 17 then 'Anulata'
                else 'Acceptata'
            end
        );
    commit;
    if ora not between 8 and 17 then
        raise_application_error(-20000, 'Nu sunt permise modificari asupra bazei de date in orele in care nu se lucreaza.');
    end if;
end;
/
--Cerinta 13
--Header
create or replace package Inventar_Evenimente as
    type Expus is record(
        exponat_id exponat.exponat_id%type,
        galerie_id galerie.galerie_id%type,
        event_id eveniment.event_id%type
    );
    type Restaurare is record(
        exponat_id exponat.exponat_id%type,
        data_inceput date,
        durata number
    );
--Folosit drept tipul listei returnate de functiei nr_exp_galerie, reprezentand perechea formata dintr-un nume de galerie si nr-ul de exponate din acea galerie ce sunt eligibile pentru un anumit eveniment 
    type returnare is record(
        nr_exponate number,
        nume_galerie galerie.nume%type);
    type tablou_returnare is table of returnare;
    type tablou_expus is table of Expus;
    type tablou_restaurare is table of Restaurare;
    lista_expus tablou_expus:=tablou_expus();
    list_restaurare tablou_restaurare:=tablou_restaurare();
    procedure resetare_tablouri;
    procedure adauga_expus (id_exp in exponat.exponat_id%type,
                            id_gal in galerie.galerie_id%type,
                            id_event in eveniment.event_id%type);
    procedure setare_restaurare (nume in categorie.nume_categorie%type,
                                inceput in date,
                                dur in number);
    procedure raport(numele in eveniment.nume%type);
    function nr_exp_event(id_event in eveniment.event_id%type) return number;
    function nr_exp_galerie(id_event in eveniment.event_id%type) return tablou_returnare;
    function eligibil (id_event eveniment.event_id%type,
                        data_s in date,
                        data_t in date) return number;
end Inventar_Evenimente;
/
--Body
create or replace package body Inventar_Evenimente as
--Utilizata pentru golirea celor doua colectii 
    procedure resetare_tablouri is 
    begin 
        list_restaurare:=tablou_restaurare();
        lista_expus:=tablou_expus();
    end resetare_tablouri;
--Toate exponatele ce sunt din categoria primita ca parametru sunt programate pentru restaurare
    procedure setare_restaurare (nume in categorie.nume_categorie%type,
                                inceput in date,
                                dur in number) is
        numele categorie.nume_categorie%type;
        numar number;
    begin
        numele:=nume;
        select count(*) into numar from categorie where trim(upper(nume_categorie))=trim(upper(nume)));
        if numar=0 then
            raise no_data_found;
        end if;
        for e in (select exponat_id from exponat where trim(upper(nume_categorie))=trim(upper(numele))) loop
            list_restaurare.extend;
            list_restaurare(list_restaurare.count):=restaurare(
                exponat_id => e.exponat_id,
                data_inceput => inceput,
                durata => dur);
        end loop;
    exception
        when no_data_found then
            dbms_output.put_line('Nu exista categorie cu numele '||nume);
    end setare_restaurare;
--Se adauga in tabloul expus daca este eligibil respectivul exponat
    procedure adauga_expus (id_exp in exponat.exponat_id%type,
                            id_gal in galerie.galerie_id%type,
                            id_event in eveniment.event_id%type) is
    begin
        for i in 1..list_restaurare.count loop
            if list_restaurare(i).exponat_id=id_exp then
                if eligibil (id_event, list_restaurare(i).data_inceput, list_restaurare(i).data_inceput+list_restaurare(i).durata) =0 then
                    return;
                end if;
            end if;
        end loop;
        lista_expus.extend;
        lista_expus(lista_expus.count):=expus(id_exp,id_gal,id_event);
    end adauga_expus;
--Se returneaza nr-ul de exponate valabile pentru evenimentul primit ca parametru
    function nr_exp_event(id_event in eveniment.event_id%type) return number as
    contor number:=0;
    begin
        for i in 1..lista_expus.count loop
            if lista_expus(i).event_id=id_event then
                contor:=contor+1;
            end if;
        end loop;
        return contor;
    end nr_exp_event;
--Se verifica daca un exponat este eligibil pentru un anumit eveniment
    function eligibil (id_event in eveniment.event_id%type,
                        data_s in date,
                        data_t in date) return number is
        sfarsit date;
        inceput date;
    begin
        select data_inceput,data_incheiere into inceput,sfarsit from eveniment where event_id=id_event;
        if not ((inceput between data_s and data_t) or (sfarsit between data_s and data_t) or (data_s between inceput and sfarsit) or (data_t between inceput and sfarsit)) then
            return 1;
        else 
            return 0;
        end if;
    end eligibil;
--Se returneaza o lista compusa din perechi de numele galeriei si nr-ul exponatelor valide de la acea galerie
    function nr_exp_galerie(id_event in eveniment.event_id%type) return tablou_returnare is
        lista_returnare tablou_returnare:=tablou_returnare();
        numar number;
    begin
        for g in (select ge.galerie_id, gal.nume from galerie_eveniment ge join galerie gal on ge.galerie_id=gal.galerie_id where event_id=id_event) loop
            numar:=0;
            for i in 1..lista_expus.count loop
                if lista_expus(i).galerie_id=g.galerie_id and  lista_expus(i).event_id=id_event then
                    numar:=numar+1;
                end if;
            end loop;
            lista_returnare.extend;
            lista_returnare(lista_returnare.count):=returnare(numar,g.nume);
        end loop;
        return lista_returnare;
    end;
--Procedura in care sunt folosite functiile nr_exp_event si nr_exp_galerie si se prezinta un raport al nr-ului de exponate si nr de exponate per fiecare galerie pentru un eveniment primit ca parametru 
    procedure raport(numele in eveniment.nume%type) as
        id_event eveniment.event_id%type;
        lista_returnare tablou_returnare:=tablou_returnare();
    begin
        select event_id into id_event from eveniment e where e.nume=numele;
        dbms_output.put_line('Evenimentul '||numele||' are '||nr_exp_event(id_event)||' exponate ce vor putea fi vazute.');
        dbms_output.put_line('Acestea sunt galeriile si cate exponate au fiecare: ');
        lista_returnare:=nr_exp_galerie(id_event);
        for i in 1..lista_returnare.count loop
            dbms_output.put_line('Galerie:'||lista_returnare(i).nume_galerie||' Nr: '||lista_returnare(i).nr_exponate);
        end loop;
    exception
        when no_data_found then
            dbms_output.put_line('Nu exista eveniment cu acest nume.');
        when too_many_rows then
           dbms_output.put_line('Prea multe evenimente cu acelasi nume.');
    end raport;
end Inventar_Evenimente;
/
--Bloc testare exemplu
declare
    nume_categorie categorie.nume_categorie%type:='Sculptura sec XIX';
    nume_eveniment eveniment.nume%type:='Eveniment';
    id_eveniment eveniment.event_id%type;
    d date:=to_date('10-MAY-24','DD-MON-YY');
    durata number:=20;
begin
    Inventar_Evenimente.resetare_tablouri;
    select event_id into id_eveniment from eveniment where nume=nume_eveniment;
    Inventar_Evenimente.setare_restaurare(nume_categorie,d,durata);
    for g in (select galerie_id from galerie_eveniment where event_id=id_eveniment) loop
        for r in (select e.exponat_id as idul from exponat e where e.galerie_id=g.galerie_id) loop
            Inventar_Evenimente.adauga_expus(r.idul,g.galerie_id,id_eveniment);
        end loop;
    end loop;
    Inventar_Evenimente.raport(nume_eveniment);
exception
    when no_data_found then
        dbms_output.put_line('Nu exista eveniment cu numele '||nume_eveniment);
end;
/


