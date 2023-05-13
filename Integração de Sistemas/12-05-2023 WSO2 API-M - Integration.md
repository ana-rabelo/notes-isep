### <span style="color:#465867"><b>Antes de começar...</b></span>
1. Vá ao [WSO2 Micro Integrator web page](https://wso2.com/integration/micro-integrator/#) e baixe o Micro Integrator como um ZIP.
2. Opcionalmente, vá até a página do [API Manager Tooling](https://wso2.com/api-management/tooling/) e baixe o WSO2 Integration Studio.
3. Baixe os arquivos [sample files](https://apim.docs.wso2.com/en/4.2.0/assets/attachments/quick-start-guide/mi-qsg-home.zip). Iremos nos referir a eles como `<mi-qsg-home>`.
4. Baixe o [curl](https://curl.haxx.se/) ou qualquer ferramenta similar que pode chamar o endpoint HTTP.
5. Opcionalmente, vá ao [WSO2 API Manager website](https://wso2.com/api-management/), clique em **TRY IT NOW**, então clique **Zip Archive** para baixar o API Manager.

### <span style="color:F46036"><b>O que iremos construir</b></span>
Esse é um cenário simples de orquestration. O cenário é sobre o sistema de uma clínica de saúde básico onde o Micro Integrator é usado para integrar dois serviços de hospital back-end para fornecer informações ao cliente.
Muitos centros de saíde tem um sistema que é usado para marcar consultas. Para checar a disponibilidade dos médicos para um determinado horário, usuários geralmente precisam visitar os hospitais ou utilizar cada e todo sistema online que é exclusivo de um centro de saúde específico. Aqui, nós estamos tornando mais fácil para os pacientes orquestrando esses sistemas isolados para cada provedor de centro de saúde e utilizando uma única interface para os usuários.

![[Pasted image 20230512224253.png]]

> [!tip] 
> **export** `<mi-qsg-home>/HealthcareIntegrationProject` para o Integration Studio para visualizar a estrutura do projeto.

