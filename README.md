# AS\_U3\_EXAMEN\_PRACTICO

# Informe de Auditoría de Sistemas – DevIA360

## 1. Resumen Ejecutivo

### 1.1 Propósito de la Auditoría

La presente auditoría tiene como fin evaluar la seguridad, la eficiencia operativa y la conformidad con buenas prácticas del entorno automatizado de despliegue continuo **Chef\_Vagrant\_Wp** empleado por **DevIA360**. El análisis abarca el código fuente, scripts de aprovisionamiento, configuraciones declarativas y evidencia obtenida durante la ejecución práctica del entorno en laboratorio.

### 1.2 Alcance Técnico Resumido

* Se desplegaron tres máquinas virtuales: `wordpress`, `database`, y `proxy` en la red privada `192.168.56.0/24` mediante `vagrant up`.
* Se revisaron configuraciones del `Vagrantfile`, archivo `.env` y data bags de Chef para identificar valores inseguros.
* Se ejecutaron pruebas funcionales y técnicas mediante `tests.sh` y `Serverspec`.

**📌 Evidencia:**

* [Anexo A – `vagrant status`](./evidencias/anexo_a_vagrant_status.png)
* [Anexo B – Pantalla de WordPress](./evidencias/anexo_b_wp_pantalla.png)

### 1.3 Principales Hallazgos

1. Exposición de credenciales sensibles en texto plano (.env, data bags)
   🥐 [Anexo D](./evidencias/anexo_d_credenciales_env.png)
2. Puertos abiertos sin restricciones y firewall desactivado
   🥐 [Anexo C](./evidencias/anexo_c_puertos_abiertos.png)
3. Falta de registros persistentes en logs del entorno
   🥐 [Anexo F](./evidencias/anexo_f_logs_ausentes.png)
4. Versiones de software obsoletas (Ubuntu 20.04, PHP 7.x, MySQL 5.x)
   🥐 [Anexo E](./evidencias/anexo_e_versiones_software.png)
5. Entorno sin segmentación dev/test/prod
   🥐 [Anexo G](./evidencias/anexo_g_sin_ambientes.png)
6. Cobertura limitada de pruebas (tests.sh básicos)
   🥐 [Anexo H](./evidencias/anexo_h_tests_vacios.png)

---

## 2. Objetivos de la Auditoría

### 2.1 Objetivo General

Evaluar los procesos y configuraciones asociados al entorno Chef\_Vagrant\_Wp con el objetivo de identificar riesgos de seguridad, deficiencias operativas y el grado de cumplimiento con normas y buenas prácticas TI.

### 2.2 Objetivos Específicos

1. Verificar la seguridad de la información gestionada por el entorno.
2. Evaluar los mecanismos de respaldo y continuidad del servicio.
3. Analizar los procesos de gestión de cambios y configuraciones.
4. Comprobar el cumplimiento de estándares como ISO 27001 y OWASP DevSecOps.
5. Identificar debilidades en pruebas, control de versiones y separación de ambientes.

---

## 3. Examen y Hallazgos

### 3.1 Configuración del Entorno

```bash
git clone https://github.com/devia360/Chef_Vagrant_Wp.git
cd Chef_Vagrant_Wp
vagrant plugin install dotenv --plugin-version 3.0.0
vagrant up
```

Se desplegaron correctamente las VMs con acceso a WordPress en `http://localhost:8080`.

### 3.2 Hallazgos Relevantes

| # | Riesgo Identificado                      | Evidencia                                              |
| - | ---------------------------------------- | ------------------------------------------------------ |
| 1 | Credenciales sensibles en texto plano    | [Anexo D](./evidencias/anexo_d_credenciales_env.png)   |
| 2 | Puertos abiertos sin restricciones       | [Anexo C](./evidencias/anexo_c_puertos_abiertos.png)   |
| 3 | Falta de logs persistentes               | [Anexo F](./evidencias/anexo_f_logs_ausentes.png)      |
| 4 | Software obsoleto                        | [Anexo E](./evidencias/anexo_e_versiones_software.png) |
| 5 | Entorno no segmentado (dev/test/prod)    | [Anexo G](./evidencias/anexo_g_sin_ambientes.png)      |
| 6 | Tests básicos sin cobertura de seguridad | [Anexo H](./evidencias/anexo_h_tests_vacios.png)       |
| 7 | Respaldos no verificados                 | [Anexo I](./evidencias/anexo_i_backups.png)            |

---

## 4. Matriz de Riesgos

| Riesgo                          | Causa (Anexo) | Impacto | Probabilidad | Nivel   |
| ------------------------------- | ------------- | ------- | ------------ | ------- |
| Credenciales expuestas          | Anexo D       | Alto    | Alta         | Crítico |
| Puertos sin restricciones       | Anexo C       | Medio   | Alta         | Alto    |
| Falta de registros de auditoría | Anexo F       | Medio   | Alta         | Alto    |
| Software obsoleto               | Anexo E       | Alto    | Media        | Alto    |
| Entorno no segmentado           | Anexo G       | Alto    | Media        | Alto    |
| Tests limitados                 | Anexo H       | Medio   | Media        | Medio   |
| Backups no verificados          | Anexo I       | Medio   | Media        | Medio   |

---

## 5. Recomendaciones

| # | Recomendación                                        | Prioridad        |
| - | ---------------------------------------------------- | ---------------- |
| 1 | Cifrar variables y usar Chef Vault o HashiCorp Vault | Alta (0-30 días) |
| 2 | Activar firewall (ufw/iptables) y restringir puertos | Alta             |
| 3 | Habilitar y centralizar logs rsyslog                 | Alta             |
| 4 | Actualizar paquetes y configurar parches automáticos | Media            |
| 5 | Separar entornos (perfiles por rama/VM/environment)  | Media            |
| 6 | Incluir tests negativos y de seguridad               | Media            |
| 7 | Automatizar backups y validar recuperación           | Media            |

---

## 6. Anexos

| Anexo | Descripción                                           | Archivo Evidencia                                                                |
| ----- | ----------------------------------------------------- | -------------------------------------------------------------------------------- |
| A     | Estado de máquinas con `vagrant status`               | [anexo\_a\_vagrant\_status.png](./evidencias/anexo_a_vagrant_status.png)         |
| B     | WordPress accesible en `http://localhost:8080`        | [anexo\_b\_wp\_pantalla.png](./evidencias/anexo_b_wp_pantalla.png)               |
| C     | Puertos abiertos sin restricciones                    | [anexo\_c\_puertos\_abiertos.png](./evidencias/anexo_c_puertos_abiertos.png)     |
| D     | Credenciales sensibles en `.env` y `data_bags`        | [anexo\_d\_credenciales\_env.png](./evidencias/anexo_d_credenciales_env.png)     |
| E     | Software obsoleto (`php -v`, `mysql --version`, etc.) | [anexo\_e\_versiones\_software.png](./evidencias/anexo_e_versiones_software.png) |
| F     | Falta de logs persistentes                            | [anexo\_f\_logs\_ausentes.png](./evidencias/anexo_f_logs_ausentes.png)           |
| G     | Entorno no segmentado (sin perfiles por ambiente)     | [anexo\_g\_sin\_ambientes.png](./evidencias/anexo_g_sin_ambientes.png)           |
| H     | Cobertura limitada de pruebas                         | [anexo\_h\_tests\_vacios.png](./evidencias/anexo_h_tests_vacios.png)             |
| I     | Respaldos no verificados                              | [anexo\_i\_backups.png](./evidencias/anexo_i_backups.png)                        |

---

> 📎 **Repositorio creado como parte del Examen Práctico de Auditoría - Unidad 3**

[Repositorio: AS\_U3\_EXAMEN\_PRACTICO](https://github.com/usuario/AS_U3_EXAMEN_PRACTICO)

---
