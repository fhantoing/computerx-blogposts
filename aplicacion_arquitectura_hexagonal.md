# Aplicación del concepto de Arquitectura Hexagonal

## Arquitectura Hexagonal

La Arquitectura Hexagonal o arquitectura de Puertos y Adaptadores hace parte del concepto de Arquitectura Limpia en el desarrollo de 
Software.

Las Arquitecturas Limpias permiten la separación de las responsabilidades del Sistema mediante capas y reglas claras de comunicación 
(dependencias) entre ellas.

El desarrollo de un sistema basado en una Arquitectura Limpia tendrá menos grado de acomplamiento del Dominio de la aplicación con elementos 
externos y permitirá además:

- Independencia de la Interfaz gráfica de Usuario (UI)
- Independencia de la Base de datos
- Mayor tolerancia a cambios
- Mayor grado de Reuzabilidad
- Mayor cobertura en pruebas (Testeable)
- Mantenibilidad
- Escalabilidad

Algunos problemas que ayuda a resolver una Arquitectura Limpia:

- Dificultad en la automatización de pruebas
- Dificultad en el cambio de Uso de la Aplicación
- Dificultad en la integración con otras Aplicaciones o Sistemas
- Alto Acoplamiento con sistemas externos como base de datos, legados, UI, etc

La Arquitectura Hexagonal propone separar la lógica del Negocio (dominio) del resto del Sistema. El dominio se representa como una API con 
contratos bien detallados denominados Puertos. Cada lado de la figura del Hexágono representaría un Puerto de la aplicación.

- Un puerto es una Interfaz o un contrato público bien definido.
- Un Adaptador es la especialización de un puerto para un contexto o caso de uso en específico.

Capas básicas de la arquitectura: 

- Aplicación
- Dominio
- Infraestructura

Ejemplo de una organización de directorios con Arquitectura Hexagonal:

```java
application\ # Implementación de Casos de Uso
domain\ # Lógica de Negocio
    exception\
    model\
infraestructure\ # Casos de Uso para integración con BD, WS, Servicios HTTP
    adapters\ # Implementación de Interfaces, Controladores
        input\ 
        output\ 
    ports\ # Interfaces
        input\  
        output\
```

Ejemplo de implementación de un Caso de Uso de Consulta a BD:

```java
public class CarsService implements InputPortCarsUseCase{

    @Autowired
    private CarsOutputPort carsOutputPort;

    @Override
    public List<Car> getListCarById(Integer id){
        return carsOutPort.getListCarById(id);
    }
}
```

## Recomendaciones

- La orientación de su proyecto hacia una organización de Arquitectura Limpia le dará orden y además facilidad para la automátización y 
  cobertura de pruebas.
- Debe tener claro cuantás capas y subcapas es necesario implementar dado que no hay restricciones o limites en la organización y dependerá 
  también de la complejidad del proyecto.
- Puede usar una Arquiectura Limpia para organizar cualquier proyecto de Software independiente de la tecnología de su proyecto. 
- Elija siempre la simplicidad y el orden.

```javascript
Estas notas son gracias a:
- Artículos en Internet de referencia:
    https://pandemoniodigital.es/arquitectura/2020/12/21/arquitectura-hexagonal-spring-boot.html
    https://www.sensedia.com.es/post/uso-del-patron-de-arquitectura-hexagonal
    https://medium.com/@edusalguero/arquitectura-hexagonal-59834bb44b7f
- Experiencias propias de Laboratorio

Puede consultar Github -> https://github.com/fhantoing/computerx-blogposts
Palabras clave: Arquitectura Hexagonal, Arquitectura Limpia, Microservicios, Java, Spring Boot.
```
