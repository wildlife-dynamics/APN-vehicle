# Vehicle

Single-vehicle activity dashboard for Argentina's national parks agency (Administración de Parques Nacionales — APN). It pulls every "Libro Diario Vehicular" (Vehicle Daily Log) event from EarthRanger over a time range, filters to one vehicle by licence plate (`patente`), extracts the per-journey details recorded in the field (odometer readings, fuel loaded, fuel type, occupants, purpose, origin/destination, driver), and reports that vehicle's usage over the period.

## Dashboard widgets

| Widget | Description |
|--------|-------------|
| Total de desplazamientos | Total number of logged journeys for the selected vehicle |
| Kilómetros totales recorridos | Sum of distance travelled (arrival odometer − departure odometer) across all journeys |
| Total de combustible consumido | Total fuel loaded across all journeys, as a quantity in litres |
| Promedio de ocupantes | Average number of occupants per journey |
| Kilómetros recorridos en el tiempo | Monthly bar chart of distance travelled |
| Ranking de choferes | Table of drivers ranked by total km driven and journey count |
| Uso del Vehículo | Full trip log: start/end date, fuel loaded, fuel type, purpose, origin, destination, distance. Sortable, filterable, downloadable |
| Recargas de combustible | Fuel-refill log: date, service station, fuel type, litres loaded, person responsible. Sortable, filterable, downloadable |

This workflow has no groupers — all widgets show data for the single selected vehicle over the configured time range in a single dashboard view.

## Selecting a vehicle

The `patente` parameter (a free-text vehicle plate, e.g. `FMC-991`) filters the fetched events down to one vehicle before any of the stats or tables are computed. There is one dashboard per plate; running the workflow for a different vehicle means changing this parameter.

## Data extraction

Each `libro_diario_vehicular_v2` event stores its details as free-form fields (in Spanish) inside `event_details`. The workflow extracts each field into its own column: `Patente` (plate), `KM_de_salida`/`Km_de_llegada` (departure/arrival odometer), `Litros_cargados` (fuel loaded), `Tipo_de_combustible` (fuel type), `Pasajeros` (occupant list, reduced to a count), `Motivo_del_desplazamiento` (trip purpose), `Origen_del_desplazamiento`/`Destino` (origin/destination), `Regreso` (return time), `Chofer` (driver), `Forma_de_recarga`/`Responsable_de_la_recarga` (refuel method/responsible party). Distance per journey is computed as arrival odometer minus departure odometer.

## Requirements

[pixi](https://pixi.sh) is required for environment and dependency management. You will also need an EarthRanger connection configured for the `apn` data source.
