use electric_vehicle;

#Query 1 --
CREATE OR REPLACE VIEW electric_vehicles_type_view AS 
SELECT
    model_name,
    electric_vehicle_type_description,
    electric_range,
    base_MSRP
FROM 
    vehicles
    JOIN models USING (model_id)
    JOIN electric_vehicle_types USING (electric_vehicle_type_id)
WHERE 
    base_MSRP > 0
ORDER BY 
    electric_range DESC, base_MSRP DESC
LIMIT 1000;

SELECT * FROM electric_vehicles_type_view;


#Query 2 -- 
CREATE OR REPLACE VIEW Average_electric_range AS 
SELECT
    model_name,
	AVG(electric_range)
FROM 
    vehicles
    JOIN models USING (model_id)
    JOIN electric_vehicle_types USING (electric_vehicle_type_id)
WHERE 
    electric_range > 0
GROUP BY 
	model_name, electric_range
ORDER BY 
    electric_range ASC;
    
SELECT * FROM Average_electric_range;

#Query 3 --
CREATE OR REPLACE VIEW model_total_sales AS 
SELECT 
    manufacturer_name,
    model_name,
    COUNT(AVG(registration_id)) AS total_sales
FROM 
    manufacturers 
    JOIN models USING (manufacturer_id)
    JOIN vehicles USING (model_id)
GROUP BY 
    manufacturer_name, model_name
ORDER BY 
    manufacturer_name, total_sales DESC;
    
SELECT * FROM model_total_sales;

#Query 4 --
CREATE OR REPLACE VIEW max_coordinates AS 
SELECT MAX(CONCAT(longitude, ", ", latitude)) AS coordinates, location_id
FROM locations
WHERE location_id BETWEEN 100 AND 300
  AND location_id IN (
    SELECT location_id
    FROM locations
    GROUP BY location_id
  )
GROUP BY location_id
ORDER BY COUNT(CONCAT(longitude, latitude));

#Query 5 --
CREATE OR REPLACE VIEW total_sales_by_city AS 
SELECT
    city_name,
    manufacturer_name, 
    model_name, 
    COUNT(registration_id) AS total_sales
FROM
    cities
JOIN 
    registrations USING (city_id)
JOIN 
    vehicles USING (registration_id)
JOIN 
    models USING (model_id)
JOIN 
    manufacturers USING (manufacturer_id)
GROUP BY 
    city_name, manufacturer_name, model_name;

SELECT * FROM total_sales_by_city;

    


