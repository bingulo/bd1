# Resumo
1. Entidade regular
2. Atributo multivalorado
3. Atributo composto
4. Entidade fraca
5. Relacionamento 1:1
6. Relacionamento 1:N
7. Relacionamento N:N
8. Relacionamento n-ário
9. Agregação
10. Generalização

# Exercícios
## 1
- R1:  
Paciente(\underline{código}, nome, sexo, dta\_nascimento, endereço)  
Material(\underline{código}, nome, preço\_unit, quant\_estoque, fornecedor)  
Cirurgia(\underline{código}, local, data, duração, custo\_total)  
Médico(\underline{código}, nome, fone)  
Especialidade(\underline{código}, nome, custo)  

- R10, R9, R8, R5, R4, R3, R2: ok.

- R6:  
Cirurgia(\underline{código}, local, data, duração, custo\_total, código\_paciente$^{CE})  
Médico(\underline{código}, nome, fone, código\_especialidade$^{CE}$)  

- R7:  
Material\_Cirurgia(\underline{código\_materia}, \underline{código\_cirurgia}, qtd)  
Médico\_Cirurgia(\underline{código\_médico}, \underline{código\_cirurgia}, tempo)  

**Final:**  
Material\_Cirurgia(\underline{código\_materia}, \underline{código\_cirurgia}, qtd)  
Médico\_Cirurgia(\underline{código\_médico}, \underline{código\_cirurgia}, tempo)  
Cirurgia(\underline{código}, local, data, duração, custo\_total, código\_paciente$^{CE}$)  
Médico(\underline{código}, nome, fone, código\_especialidade$^{CE}$)  
Especialidade(\underline{código}, nome, custo)  
Paciente(\underline{código}, nome, sexo, dta\_nascimento, endereço)  
Material(\underline{código}, nome, preço\_unit, quant\_estoque, fornecedor)  



