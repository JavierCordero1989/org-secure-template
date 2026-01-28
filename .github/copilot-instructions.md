# Instrucciones para GitHub Copilot

Estas instrucciones son OBLIGATORIAS para todo el código generado, donde se debe priorizar la seguridad, el cumplimiento regulatorio, la trazabilidad y la mantenibilidad.

---

## 1. Reglas Generales de Codificación
Alineado a:
- ISO/IEC 25010: define un modelo integral para evaluar la calidad del software y los sistemas
- ISO 27001: estándar internacional líder para gestionar la seguridad de la información

- Generar código explícito, legible y mantenible.
- Evitar atajos, soluciones temporales o hacks.
- No duplicar lógica.
- Aplicar principios SOLID y separación de responsabilidades.
- Preferir claridad sobre optimización prematura.
- Documentar decisiones relevantes en el código.
---

## 2. Manejo de Datos Sensibles
Alineado a:
- ISO 27001: estándar internacional líder para gestionar la seguridad de la información
- PCI DSS (Payment Card Industry Data Security Standard): conjunto de normas de seguridad obligatorias para cualquier entidad que almacene, procese o transmita datos de tarjetas de crédito o débito
- GDPR (General Data Protection Regulation): Establece un control estricto sobre el consentimiento, la seguridad y el tratamiento de datos, con sanciones graves

- NUNCA:
  - Hardcodear credenciales, tokens, secretos, claves, certificados.
  - Incluir datos personales o financieros en logs.
  - Retornar información sensible en mensajes de error.

- SIEMPRE:
  - Usar variables de entorno o servicios de gestión de secretos.
  - Enmascarar datos sensibles (ej: ****1234).
  - Validar el uso mínimo de datos (data minimization).

Ejemplos de datos sensibles:
- Passwords
- Tokens
- Claves API
- Números de cuenta
- PAN, CVV, PIN
- Identificadores personales

---

## 3. Validación de Entradas y Salidas
Alineado a:
- OWASP Top 10: documento de los diez riesgos de seguridad más importantes en aplicaciones web según la Fundación OWASP

- Validar TODA entrada externa:
  - APIs
  - Archivos
  - Formularios
  - Mensajes
- Rechazar entradas inválidas explícitamente.
- No confiar en validaciones del cliente.
- Usar listas blancas (allowlists) cuando sea posible.

Prevención obligatoria contra:
- SQL Injection
- Command Injection
- XSS
- Path Traversal
- Deserialización insegura

---

## 4. Autenticación y Autorización
Alineado a:
- NIST SSDF: Secure Software Development Framework, Proporciona un conjunto estructurado de prácticas destinadas a reducir las vulnerabilidades, mejorar la resiliencia y alinear la seguridad del software con los objetivos organizacionales.
- OWASP: Open Web Application Security Project, proyecto dedicado a determinar y combatir las causas que hacen que el software sea inseguro.

- No implementar autenticación propia si existe un mecanismo corporativo.
- Validar autorización en cada operación sensible.
- Aplicar principio de mínimo privilegio.
- No asumir permisos por contexto.
- Separar claramente:
  - Autenticación
  - Autorización
  - Identidad

---

## 5. Manejo de Errores y Excepciones
(Alineado a ISO 27001, OWASP)

- No exponer:
  - Stack traces
  - Mensajes internos
  - Detalles de infraestructura
- Usar mensajes controlados hacia el usuario.
- Registrar errores técnicos SOLO en logs internos.
- Clasificar errores (cliente, negocio, sistema).

---

## 6. Logging y Auditoría
(Alineado a ISO 27001, COBIT)

- Incluir logs estructurados para:
  - Operaciones críticas
  - Cambios de estado
  - Eventos de seguridad
- Los logs deben:
  - Ser trazables
  - Tener identificadores de correlación
  - NO contener datos sensibles
- Facilitar auditoría y análisis forense.

---

## 7. Uso de Criptografía
(Alineado a ISO 27001, PCI DSS)

- No implementar algoritmos criptográficos manualmente.
- Usar librerías estándar aprobadas.
- Usar algoritmos fuertes y actuales.
- No usar:
  - MD5
  - SHA1
  - Cifrados obsoletos
- Proteger claves criptográficas adecuadamente.

---

## 8. Dependencias y Librerías
(Alineado a NIST SSDF, Supply Chain Security)

- No sugerir librerías no mantenidas o inseguras.
- Evitar dependencias innecesarias.
- Preferir librerías:
  - Oficiales
  - Bien documentadas
  - Con mantenimiento activo
- No copiar código de fuentes no confiables.

---

## 9. Configuración y Ambientes
(Alineado a ISO 27001, DevSecOps)

- Separar configuración del código.
- No asumir valores por defecto inseguros.
- Soportar múltiples ambientes (dev, test, prod).
- No incluir configuraciones específicas de producción en el código.

---

## 10. Pruebas
(Alineado a ISO 25010, NIST SSDF)

- Incluir pruebas unitarias para lógica crítica.
- Considerar pruebas de seguridad cuando aplique.
- No desactivar validaciones para “facilitar pruebas”.
- Facilitar automatización de pruebas.

---

## 11. Restricciones Explícitas
(Alineado a Cumplimiento Regulatorio)

- No sugerir soluciones que violen políticas internas.
- No generar código que eluda controles de seguridad.
- No generar puertas traseras o accesos ocultos.
- No sugerir deshabilitar validaciones de seguridad.

---

## 12. Estándar de Mensajes de Commit
(Alineado a Conventional Commits, ISO 27001, COBIT)

Los mensajes de commit son OBLIGATORIOS y deben seguir el estándar
**Conventional Commits** para asegurar trazabilidad, auditoría y automatización.

### Formato obligatorio

`<type>(<scope>): <descripción corta>`

Ejemplo:
- feat(auth): agregar validación de token JWT
- fix(api): corregir validación de entrada nula
- chore(ci): actualizar pipeline de análisis estático

### Tipos permitidos
- feat: nueva funcionalidad
- fix: corrección de defecto
- refactor: cambio interno sin impacto funcional
- perf: mejora de rendimiento
- test: pruebas
- docs: documentación
- chore: tareas técnicas
- ci: cambios en integración continua
- build: cambios de build o dependencias
- revert: reversión de cambios

### Reglas obligatorias
- Usar tiempo presente (“agregar”, no “agregado”).
- Descripción clara y concisa.
- No exceder 72 caracteres en la línea principal.
- No usar mensajes genéricos:
  - ❌ "fix"
  - ❌ "changes"
  - ❌ "update"
- Incluir scope cuando aplique (módulo, componente, servicio).

### Cambios críticos
Para cambios que rompen compatibilidad:

BREAKING CHANGE: descripción del impacto

### Seguridad
- Commits relacionados con seguridad deben indicarlo explícitamente:
  - fix(security): corregir validación de permisos
  - feat(auth): fortalecer autenticación

### Restricciones
- No incluir datos sensibles en mensajes de commit.
- No incluir secretos, claves, usuarios o datos productivos.

---

## 13. Revisión Asistida de Pull Requests

Cuando se solicite ayuda para revisar un Pull Request, debes:

- Analizar los mensajes de commit y verificar que cumplan el estándar Conventional Commits.
- Indicar claramente qué commits no cumplen y por qué.
- Revisar la descripción del PR y señalar campos faltantes.
- Advertir sobre posibles incumplimientos de:
  - Seguridad
  - Manejo de datos sensibles
  - Validación de entradas
- NO bloquear ni rechazar el Pull Request.
- Proveer recomendaciones claras y accionables para corregir los puntos detectados.

El objetivo es orientar al desarrollador antes de completar el Pull Request.

---

## 14. Principio Final
Ante duda:
- Priorizar seguridad sobre conveniencia.
- Priorizar cumplimiento sobre velocidad.
- Priorizar claridad sobre complejidad.
