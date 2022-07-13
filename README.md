# Analisis-con-SQL

###Alimento de cada animal segun su nombre particular
SELECT ANIMAL.Name, ANIMAL.Species, FEEDING.FoodType 
FROM ANIMAL INNER JOIN FEEDING 
on ANIMAL.Species = FEEDING.Species
;

###Animales para cada clase e inversion por alimento
SELECT COUNT(ANIMAL.Name) as Cantidad_Animales, ANIMAL.Class, sum(FEEDING.FoodPrice) as Inversion_Alimento 
FROM ANIMAL INNER JOIN FEEDING 
on ANIMAL.Species = FEEDING.Species
GROUP BY ANIMAL.Class
ORDER BY COUNT(ANIMAL.Name) DESC;

## Hay mayor cantidad de mamíferos en el zoológico (12), solo 2 aves y 2 reptiles. Los que requieren mayor inversión son los escifozoos y
## y los de clases Gaastropoda. De los que requieren menor inversión, son los mamíferos y reptiles.

###Personas que alimentan a los animales
SELECT EMPLOYEE.EmployeeFullName, ZSKILLS.Skill
FROM EMPLOYEE INNER JOIN ZOOKEEPER 
on EMPLOYEE.EmployeeID = ZOOKEEPER.ZEmpID
INNER JOIN ZSKILLS 
on ZOOKEEPER.KeeperID = ZSKILLS.KSkillID
WHERE ZSKILLS.Skill LIKE '%Feed%';

## Cornelius, Patricia Butler, Pamela Ida y Wendell Gray son los encargados de alimentar a los animales. 

###Alimentos registrados
SELECT COUNT(FEEDING.FoodType) as Total_Alimento_Reg
FROM FEEDING;
## En total, se tienen registrados 20 tipos de alimentos

#Bonos para trabajadores que ingresaron antes del 2000
SELECT EMPLOYEE.HireDate, EMPLOYEE.EmployeeFullName, EMPLOYEE.EmployeeID 
FROM EMPLOYEE 
WHERE EMPLOYEE.HireDate BETWEEN '1950-01-01' AND '1999-12-31';
## George, Karen, Michael, Nuzhat, Patricia, Pamela, Safiya y Stephanie recibiran el bono por haber ingresado antes del anho 2000.

###Trabajador de cuidados intensivos
SELECT EMPLOYEE.EmployeeFullName, EMPLOYEE.EmployeeID 
FROM EMPLOYEE INNER JOIN ZOOKEEPER 
on EMPLOYEE.EmployeeID =ZOOKEEPER.ZEmpID 
INNER JOIN ZSKILLS 
on ZOOKEEPER.KeeperID = ZSKILLS.KSkillID 
WHERE ZSKILLS.Skill LIKE 'Intensive Care';
## El trabajador Nuzhat Zaman recibira el bono por trabajar en cuidados intensivos


###NOMBRE, SALARIO, ACTIVIDAD ESPECIFICA
SELECT EMPLOYEE.EmployeeFullName, ZOOKEEPER.Salary, ZSKILLS.Skill 
FROM EMPLOYEE INNER JOIN ZOOKEEPER 
on EMPLOYEE.EmployeeID =ZOOKEEPER.ZEmpID 
INNER JOIN ZSKILLS 
on ZOOKEEPER.KeeperID =ZSKILLS.KSkillID;


###STAFF DEL ZOOLOGICO
SELECT PERSON.FullName , OSTAFF.Department 
FROM OSTAFF INNER JOIN EMPLOYEE 
on EMPLOYEE.EmployeeID =OSTAFF.OEmpID
INNER JOIN PERSON
on PERSON.FullName =EMPLOYEE.EmployeeFullName ;
## Los trabajadores Michael Michael, Joseph Samadi, Brandon Perez, Stephanie Ross, Meera Sakshi, Safyra Irene, George, Patrick Johnson
## Dennis Simmons, Lara Gall, Karen Reed, Chris Turner, pertenecen al staff del zoologico.

###DINERO PAGADO POR CADA TIPO DE TRABAJADOR
SELECT SUM(Salary) AS Money_Paid, Department
FROM OSTAFF 
GROUP BY Department;
## Se paga 3000 $ por trabajadores de Finanzas, 2400 $ por Recursos Humanos, 5400 $ Por los de administracion, 1500 $ por trabajadores de mantenimiento, 1200 $ Por compras y 1200 $ por cuentas de pago.

###Segmentos de personas que habria que atraer
SELECT VISITOR.VisitorCategory, COUNT(VISITOR.VisitorCategory) AS Total_Por_Categoria,
SUM(VISITOR.TicketPrice) AS _Total_Recaudado
FROM VISITOR INNER JOIN PERSON
on VISITOR.VisitorFullName =PERSON.FullName
GROUP BY VisitorCategory;
## Habría que atraer más adultos, debido que son los que más asisten y tienen el valor del ticket más costoso.
## Debería desistir de los usuarios de tercera edad. Son los que menos asisten y su valor de ticket está entre los económicos

###Estado de las personas que mas van al zoologico
SELECT PERSON.US_State, COUNT(PERSON.US_State) AS Visitantes_Por_Estado, SUM(VISITOR.TicketPrice) AS Total_Recaudado
FROM PERSON INNER JOIN VISITOR 
on VISITOR.VisitorFullName = PERSON.FullName 
GROUP BY US_State
ORDER BY COUNT(PERSON.US_State) DESC;
## Los estados donde se ubican las personas que más visitan al zoológico son Massachussets y Florida.

###Ticket promedio del estado donde las personas visitan mas al zoologico
SELECT  PERSON.US_State, AVG(VISITOR.TicketPrice) AS Ticket_Promedio, COUNT(PERSON.US_State) AS Personas_Por_Estado
FROM PERSON INNER JOIN VISITOR 
on VISITOR.VisitorFullName =PERSON.FullName 
GROUP BY US_State
ORDER BY COUNT(PERSON.US_State) DESC;
## El valor del ticket promedio de los visitantes de aquellos Estados donde mas visitan al zoologico es de 3600 $ y 3200 $, respectivamente.

###Estado donde se encuentran los adultos que mas visitan el zoologico
SELECT PERSON.US_State, COUNT(VISITOR.VisitorCategory) AS Adultos_Por_Estado
FROM VISITOR INNER JOIN PERSON 
on VISITOR.VisitorFullName =PERSON.FullName 
WHERE VisitorCategory LIKE 'ADULTO/A'
GROUP BY US_State
ORDER BY COUNT(VISITOR.VisitorCategory) DESC;
## Massachussets es el estado donde se encuentran la mayor cantidad de adultos que visitan el zoológico

###Ciudad donde se encuentra la mayoria de los invitados
SELECT COUNT(VISITOR.TicketType) AS TicketDia_Por_Ciudad, PERSON.City  
FROM PERSON INNER JOIN VISITOR 
on VISITOR.VisitorFullName =PERSON.FullName 
WHERE VISITOR.TicketType LIKE 'Day'
GROUP BY City
ORDER BY COUNT(VISITOR.TicketType) DESC;
## La ciudad donde se ubican la mayoría de los invitados con ticket diario, es la ciudad de Lincoln.