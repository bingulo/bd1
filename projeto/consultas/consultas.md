---
geometry: margin=3cm
---

# Consultas G10

## Consultas simples
### 1.
**Objetivo:** Acessar os nomes dos funcionários cadastrados no banco e seus
respectivos e-mails.

**SQL:**
```SQL
SELECT  nome, email
FROM    funcionario;
```

**Resultado:**

|         nome         |          email           |
|:--------------------:|:------------------------:|
| Antronio Miguel      | am@unicentro.com         |
| Gilberto Motta       | gmota@unicentro.com      |
| Nicolau Antônio      | nicantonio@unicentro.com |
| Java Travasso        | java@unicentro.com       |
| Samambaia Trimbilina | samt@unicentro.com       |
| Sabrina Mattos       | smato@unicentro.com      |
| Juan Lopes           | juan@unicentro.com       |
| Miguel Jefferson     | miguel@unicentro.com     |


### 2.
**Objetivo:** Acessar os pacientes cadastrados no banco e todas as suas
informações (id, nome, cpf, dta_nasc, sexo)

**SQL:**
```SQL
SELECT  id, nome, cpf, dta_nasc, sexo
FROM    paciente;
```

**Resultado:**

|id |         nome          |     cpf     |  dta_nasc  | sexo  |
|:-:|:---------------------:|:-----------:|:----------:|:-----:|
| 1 | Teste                 | 49350094894 | 2000-04-24 | M     |
| 2 | Gilberto Silva        | 49350094892 | 2000-07-24 | M     |
| 3 | Salvio Krebs          | 49350094888 | 2000-09-24 | M     |
| 4 | Mauricia Souza        | 49350094895 | 2004-06-24 | F     |
| 5 | Cirilo João           | 49350094936 | 2000-07-22 | M     |
| 6 | Waushington Melo Lima | 49350094832 | 1894-07-24 | F     |

## Consultas usando junções internas (INNER JOIN)
### 1.
**Objetivo:** Acessar os diagnósticos de cada paciente.    

**SQL:**
```SQL
SELECT   p.nome, d.descricao
FROM     (atendimento_tem_diagnostico AS ad
         JOIN   paciente AS p
         ON     ad.id_pac = p.id)
         JOIN   diagnostico AS d
         ON     ad.id_dia = d.id
GROUP BY p.nome, d.descricao
ORDER BY p.nome;
```

**Resultado:**

|      nome      |      descricao     |
|:--------------:+:------------------:|
|Cirilo João    | câncer              |
|Cirilo João    | infarto             |
|Mauricia Souza | câncer              |
|Teste          | insuficiência renal |

### 2.
**Objetivo:** Saber qual médico atendeu a cada paciente e quando.

**SQL:**
```SQL
SELECT  f.nome AS medico, a.hra_inicio AS "data/hora", p.nome AS paciente
FROM    func_prof_saude_tem_atendimento fa
JOIN    func_prof_saude_medico AS fam 
ON      fam.id_func = fa.id_func
JOIN    funcionario AS f
ON      f.id = fa.id_func
JOIN    paciente p 
ON      p.id = fa.id_pac
JOIN    atendimento AS a 
ON      a.id = fa.id_ate;
```

**Resultado:**

|    medico     |      data/hora      |    paciente    |
|:-------------:|:-------------------:|:--------------:|
|Juan Lopes     | 2020-03-12 13:40:00 | Teste          |
|Juan Lopes     | 2021-03-14 14:40:00 | Salvio Krebs   |
|Juan Lopes     | 2021-03-14 19:30:00 | Mauricia Souza |
|Gilberto Motta | 2021-03-14 16:40:00 | Salvio Krebs   |
|Gilberto Motta | 2021-03-14 19:30:00 | Mauricia Souza |
|Gilberto Motta | 2021-03-15 10:30:00 | Cirilo João    |

## Consultas usando junções externas (OUTER JOIN)
### 1.
**Objetivo:** Descobrir todos os telefones de farmacêuticos disponíveis.

**SQL:**
```SQL
SELECT          f.nome, ft.telefone
FROM            func_prof_saude_farmaceutico ff
JOIN            funcionario AS f 
ON              f.id = ff.id_func
LEFT OUTER JOIN func_tel ft 
ON              ft.id_func = ff.id_func;
```

**Resultado:**

|       nome      |   telefone    |
|:---------------:|:-------------:|
|Miguel Jefferson | (15)994372687 |
|Miguel Jefferson | (31)415926535 |
|Antronio Miguel  |               |

## Consultas usando agrupamento (GROUP BY)
### 1.
**Objetivo:** Saber o salário médio dos funcionários do período noturno.

**SQL:**
```SQL
SELECT   turno, AVG(salario) "Salário médio"
FROM     funcionario
WHERE    turno = 'Noturno'
GROUP BY turno;
```

**Resultado:**

|  turno  | Salário médio         |
|:-------:|:---------------------:|
| Noturno | 4000.0000000000000000 |

### 2.
**Objetivo:** Saber a quantidade total de exames realizado para cada um dos tipos.

**SQL:**
```SQL
SELECT   tipo, count(*) AS "quantidade"
FROM     exame
GROUP BY tipo;
```

**Resultado:**

|        tipo          | quantidade  |
|:--------------------:|:-----------:|
|ultrassom             |          4  |
|ressonância magnética |          1  |
|tomografia            |          1  |

### 3.
**Objetivo:** Saber quais funcionários trabalham no período matutino.

**SQL:**
```SQL
SELECT   nome, turno
FROM     funcionario
WHERE    turno = 'Matutino'
GROUP BY funcionario.nome, funcionario.turno;
```

**Resultado:**

|     nome       |  turno   |
|:--------------:|----------|
|Antronio Miguel | Matutino |
|Gilberto Motta  | Matutino |
|Nicolau Antônio | Matutino |
|Sabrina Mattos  | Matutino |

## Consultas usando subconsultas
### 1.
**Objetivo:** Nome e cpf dos pacientes do atendimento de emergência.


**SQL:**
```SQL
SELECT  nome, cpf
FROM    paciente
WHERE   id IN
            (SELECT id_pac
             FROM   atendimento
             WHERE  tipo = 'emergencia');
```

**Resultado:**

|      nome      |     cpf     |
|:--------------:|:-----------:|
| Teste          | 49350094894 |
| Gilberto Silva | 49350094892 |
| Salvio Krebs   | 49350094888 |
| Mauricia Souza | 49350094895 |
| Cirilo João    | 49350094936 |
