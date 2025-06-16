# 🌱 FarmTech Solutions - Sistema Inteligente de Irrigação (Fase 4)

## Sumário

Este repositório contém o desenvolvimento da **Fase 4** do projeto **FarmTech Solutions**, focado na criação de um sistema inteligente de irrigação. Esta fase integra o monitoramento de sensores (umidade, temperatura, NPK) com um dashboard interativo utilizando **Streamlit** e um modelo de **Machine Learning** para previsão de necessidade de irrigação, tudo suportado por um banco de dados **SQLite**. O firmware para o **ESP32** é simulado no **Wokwi** para demonstrar a coleta de dados e a interface LCD.

---

## 1. Visão Geral do Projeto

O projeto **FarmTech Solutions** visa modernizar a agricultura através de soluções **IoT**. Nesta Fase 4, simulamos a coleta de dados de **umidade**, **temperatura** e níveis de nutrientes (**N**, **P**, **K**) usando um **ESP32**.

Esses dados, juntamente com informações de irrigação, são armazenados em um banco de dados. Um modelo de **Machine Learning** (scikit-learn) é treinado para prever a necessidade de irrigação, e um **dashboard interativo** (Streamlit) é desenvolvido para visualizar os dados, controlar o sistema (futuramente) e exibir as previsões.

---

## 2. Estrutura do Repositório

```
FarmTech-Fase4/
├── .gitignore
├── README.md
├── requirements.txt
│
├── esp32_firmware/
│   └── farmtech_esp32_wokwi.ino
│
└── python_app/
    ├── main.py
    ├── requirements.txt
    ├── farmtech_data.db
    └── irrigation_model.pkl
```

---

## 3. Firmware do ESP32 (Simulação no Wokwi)

### 3.1. Funcionalidades

- **Leitura Simulada de Sensores**: Umidade e Temperatura (DHT22), N, P, K simulados.
- **Exibição no LCD I2C**: LCD 16x2 exibe valores de sensores.
- **Comunicação Serial**: Dados enviados ao Serial Monitor/Plotter.
- **Otimização de Memória**: Uso de `uint8_t` para nutrientes, economizando memória.

### 3.2. Circuito no Wokwi

Componentes simulados:

- ESP32 DevKit C V4  
- Sensor DHT22  
- Display LCD 16x2 (I2C)  
- Potenciômetros (simulando N, P, K)

**Conexões principais**:

- `DHT22 DATA`: GPIO 15  
- `LCD I2C SDA`: GPIO 18  
- `LCD I2C SCL`: GPIO 19  

**![simlador LCD](https://github.com/user-attachments/assets/76a8e317-bb07-4616-af54-5cae02a81c77)**

---

### 3.3. Saída do Serial Monitor e Plotter (Wokwi)

**![simulador lcd 2](https://github.com/user-attachments/assets/30df5579-fb3e-487c-a856-f4f186a6ebee)**  


---

### 3.4. Exibição no LCD (Wokwi)

**![image](https://github.com/user-attachments/assets/718fe4d6-0c84-4bec-9eaa-f06c1ceb43b0)**

---

## 4. Design do Banco de Dados

Utilizamos o banco de dados `SQLite` (`farmtech_data.db`) para armazenar e analisar os dados.

### 4.1. Esquema do Banco de Dados

```sql
-- Leituras dos sensores
CREATE TABLE leituras_sensores (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
    umidade REAL NOT NULL,
    temperatura REAL NOT NULL,
    nivel_n INTEGER NOT NULL,
    nivel_p INTEGER NOT NULL,
    nivel_k INTEGER NOT NULL,
    area_id INTEGER
);

-- Previsões do modelo de ML
CREATE TABLE previsoes_ml (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp_previsao DATETIME DEFAULT CURRENT_TIMESTAMP,
    leitura_id INTEGER NOT NULL,
    necessidade_irrigacao_prevista INTEGER NOT NULL,
    confianca_modelo REAL,
    tipo_modelo_usado TEXT,
    FOREIGN KEY (leitura_id) REFERENCES leituras_sensores(id)
);

-- Ações de irrigação
CREATE TABLE acoes_irrigacao (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp_inicio DATETIME DEFAULT CURRENT_TIMESTAMP,
    timestamp_fim DATETIME,
    duracao_segundos INTEGER,
    tipo_acao TEXT NOT NULL,
    leitura_id_referencia INTEGER,
    FOREIGN KEY (leitura_id_referencia) REFERENCES leituras_sensores(id)
);
```

---

## 5. Aplicativo Streamlit e Machine Learning

### 5.1. Funcionalidades

- Dashboard interativo com gráficos históricos  
- Geração de dados simulados para testes  
- Treinamento de modelo (Regressão Logística - scikit-learn)  
- Previsão de irrigação com sliders interativos  
- Persistência do modelo (`irrigation_model.pkl`)

### 5.2. Capturas de Tela

**![image](https://github.com/user-attachments/assets/92749d93-7644-4d73-9c1a-977230e6b540)
**  
**![image](https://github.com/user-attachments/assets/690ece42-e3f9-4ed7-b7d2-f992b966a337)
**  
![image](https://github.com/user-attachments/assets/c1aca5d1-3af0-4ac1-851f-310751ff973b)
**
**![image](https://github.com/user-attachments/assets/0b7a4104-9d82-4d80-8ad6-ce0c6e6cc560)**


---

## 6. Como Rodar o Projeto

### Acesse a pasta do app Python

```bash
cd python_app
```

### Instale as dependências

```bash
pip install -r requirements.txt
```

**`python_app/requirements.txt` deve conter:**
```
pandas
scikit-learn
streamlit
matplotlib
```

### Execute o aplicativo

```bash
streamlit run main.py
```

Acesse o dashboard no navegador: [http://localhost:8501](http://localhost:8501)

---

## 7. 🎥 Vídeo de Demonstração (Entregável da Fase 4)

Assista ao vídeo demonstrando o sistema FarmTech Solutions:

📺 [LINK_DO_VIDEO_YOUTUBE](https://www.youtube.com/watch?v=[ID_DO_SEU_VIDEO])

---

## 8. Próximos Passos e Desafios Futuros

- 🔌 Integração com ESP32 físico  
- 🚰 Controle real de bomba de irrigação  
- 📊 Coleta de mais dados reais  
- 🤖 Teste de novos algoritmos de ML  
- 🖥️ Melhorias de UI/UX no dashboard

---

## 👨‍💻 Desenvolvido por

[SEU_NOME]
