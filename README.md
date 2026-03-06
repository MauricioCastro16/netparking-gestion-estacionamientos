[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/MauricioCastro16/Seminario-Estacionamientos)

# NetParking - Gestión de Estacionamientos

![.NET](https://img.shields.io/badge/.NET-9.0-purple.svg)
![ASP.NET Core](https://img.shields.io/badge/ASP.NET%20Core-MVC-blue.svg)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue.svg)

Sistema web completo para la gestión integral de playas de estacionamiento. Permite administrar plazas, controlar accesos, gestionar abonos y generar informes en tiempo real con múltiples roles de usuario.

## Características Principales

- Gestión multirol de estacionamientos (Administrador, Dueño, Playero, Conductor)
- Configuración dinámica de plazas con clasificación de vehículos
- Sistema de turnos y control de ingresos/egresos en tiempo real
- Gestión de abonos y servicios extra con tarifas personalizables
- Informes financieros y operativos con exportación PDF

## Stack Tecnológico

| Categoría | Tecnología |
|----------|------------|
| **Frontend** | Bootstrap 5, jQuery, DataTables, Leaflet.js, Chart.js |
| **Backend** | ASP.NET Core MVC (.NET 9), Entity Framework Core |
| **Base de Datos** | PostgreSQL |
| **Herramientas** | Docker, GitHub Actions, Render.com, QuestPDF |

## Arquitectura

Aplicación web MVC con arquitectura limpia separada en Controllers, Services, Data y Models. Utiliza Entity Framework Core con patrón TPT para la jerarquía de usuarios, autenticación basada en claims y contenerización Docker para despliegue. El sistema maneja cuatro roles con permisos diferenciados y flujos de trabajo específicos para cada tipo de usuario.

## Estrategia de Ramificación - GitFlow  
  
### **main**  
Rama principal y estable. Contiene únicamente versiones listas para producción.  
  
### hotfix/*  
Rama para arreglar rápido errores críticos en producción. Parte de **main** y luego se fusiona en **main** y **develop**.  
  
### release/*  
Rama para preparar una nueva versión (solo fixes y ajustes menores). Parte de **develop** y luego se fusiona en **main** y **develop**.  
  
### **develop**  
Rama de integración donde se juntan todas las nuevas funcionalidades antes de un release.  
  
### feature/*  
Rama temporal para desarrollar una nueva funcionalidad. Parte de **develop** y vuelve a **develop**.  
  
## Arquitectura por Capas (MVC+)  
  
### Controllers  
Orquestan la request → llaman servicios → devuelven View/JSON. No deberían contener reglas de negocio ni queries complejas.  
  
### Services  
Lógica de negocio. Acá van reglas, validaciones de dominio, cálculos, casos de uso (crear turno, cerrar caja, recalcular promedio, etc.). Se exponen como interfaces (p. ej. IPlayasService) e implementaciones inyectables.  
  
### Data  
Acceso a datos: AppDbContext (EF Core) y, si querés, repositorios finos para consultas específicas. El service usa el DbContext (o repos), maneja transacciones y unit of work.  
  
### Models  
Entidades (EF), Value Objects, enums. Sin dependencias de UI.  
  
### Views  
Formatos para entrada/salida (lo que recibe y devuelve el controller). Usá AutoMapper si te gusta.  
  
### Validators  
Reglas de validación de entrada (FluentValidation) separadas del controller.  

## Instalación y Uso

```bash
# Clonar repositorio
git clone https://github.com/tu-usuario/netparking-gestion-estacionamientos.git
cd netparking-gestion-estacionamientos

# Configurar variables de entorno
cp .env.example .env
# Editar .env con tus credenciales de PostgreSQL

# Restaurar dependencias
dotnet restore

# Aplicar migraciones
dotnet ef database update

# Ejecutar aplicación
dotnet run
```

```bash
# Ejecutar con Docker
docker build -f Dockerfile.simple -t netparking .
docker run -p 8080:8080 --env-file .env netparking
```
