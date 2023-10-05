# TAREA-DE-OPTIMIZACION-PROGRAMACION-NO-LINEAL

Miembros del grupo: Germán Llorente y Carlos Puigserver

El link al repositorio es el siguiente: https://github.com/Germiprogramer/TAREA-DE-OPTIMIZACION-PROGRAMACION-NO-LINEAL.git

# Propuesta Teórica del Modelo de Atribución con Programación No Lineal:

A continuación, desarrollaremos una propuesta teórica para explicar cómo este modelo de atribución basado en programación no lineal podría abordar el problema de los rendimientos decrecientes y maximizar el ROI. Esta propuesta se basará en el código proporcionado y destacará los pasos clave del proceso:

Supongamos que tienes un presupuesto total de $100,000 para invertir en publicidad para estos productos. Además, tienes datos históricos de inversiones y conversiones para tres canales de marketing (Google Ads, Facebook Ads y Twitter Ads) para estos productos. Los datos históricos son los siguientes:

Para Google Ads:

Inversiones históricas: $50,000
Conversiones atribuibles: 100

Para Facebook Ads:

Inversiones históricas: $30,000
Conversiones atribuibles: 60
Para Twitter Ads:

Inversiones históricas: $20,000
Conversiones atribuibles: 40

El objetivo es maximizar el retorno de inversión (ROI) para la publicidad en estas tres webs.



# Variables del problema

historical_investments: Matriz que contiene las inversiones históricas en publicidad para cada canal (Google Ads, Facebook Ads, Twitter Ads) para los productos (mesa, silla, lámpara).
historical_conversions: Matriz que contiene las conversiones atribuibles a cada canal para los productos.
Parámetros de las Curvas de Respuesta:

alphas: Vector que contiene los valores alfa para las curvas de respuesta de cada canal.
betas: Vector que contiene los valores beta para las curvas de respuesta de cada canal.
Presupuesto Total Disponible:

total_budget: Presupuesto total disponible para la campaña publicitaria.
Variables de Decisión:

google_budget, facebook_budget, twitter_budget: Variables que representan el presupuesto asignado a cada canal respectivamente. Estas variables son positivas, lo que significa que no se permite un presupuesto negativo.
Restricción del Presupuesto Total:

budget_constraint: Lista que contiene la restricción que asegura que la suma del presupuesto asignado a cada canal no exceda el presupuesto total disponible.
Función Objetivo (ROI):

roi: La función objetivo representa el retorno de inversión (ROI) total esperado. Utiliza las curvas de respuesta (alfa y beta) para cada canal y las conversiones históricas para calcular el ROI total.
Creación del Problema de Optimización:

problem: Se crea el problema de optimización especificando la función objetivo que se desea maximizar y las restricciones que se deben cumplir.
Resolución del Problema:

Se utiliza el solver ECOS para resolver el problema de optimización y encontrar los valores óptimos de google_budget, facebook_budget, y twitter_budget.
Resultados:

Se verifica si se encontró una solución óptima (cp.OPTIMAL). Si es así, se imprime el presupuesto óptimo asignado a cada canal y el ROI óptimo. Si no se encuentra una solución óptima, se imprime un mensaje indicando que no se pudo encontrar una solución óptima.

# CÓDIGO DEL PROBLEMA

        import cvxpy as cp
        import numpy as np
        
        # Datos históricos para cada canal (Google Ads, Facebook Ads, Twitter Ads) en la Universidad UAX
        historical_investments = np.array([[40000, 30000, 30000]])  # Inversiones en Google Ads, Facebook Ads, Twitter Ads
        historical_conversions = np.array([[80, 60, 50]])  # Conversiones atribuibles a cada canal
        
        # Parámetros de las curvas de respuesta (asumiendo valores similares a los del problema original)
        alphas = np.array([-9453.72, -8312.84, -7371.33])
        betas = np.array([8256.21, 7764.20, 7953.36])
        
        # Presupuesto total disponible
        total_budget = 100000
        
        # Variables de decisión (presupuesto asignado a cada canal para la Universidad UAX)
        google_budget = cp.Variable(pos=True)
        facebook_budget = cp.Variable(pos=True)
        twitter_budget = cp.Variable(pos=True)
        
        # Restricción de presupuesto total
        budget_constraint = [google_budget + facebook_budget + twitter_budget <= total_budget]
        
        # Función objetivo: Maximizar el ROI para la Universidad UAX
        roi = (
            (alphas[0] + betas[0] * cp.log(google_budget)) * historical_conversions[0, 0] +
            (alphas[1] + betas[1] * cp.log(facebook_budget)) * historical_conversions[0, 1] +
            (alphas[2] + betas[2] * cp.log(twitter_budget)) * historical_conversions[0, 2]
        )
        objective = cp.Maximize(roi)
        
        # Crear el problema de optimización
        problem = cp.Problem(objective, budget_constraint)
        
        # Resolver el problema
        problem.solve(solver=cp.ECOS)
        
        # Resultados
        if problem.status == cp.OPTIMAL:
            optimal_google_budget = google_budget.value
            optimal_facebook_budget = facebook_budget.value
            optimal_twitter_budget = twitter_budget.value
            optimal_roi = roi.value
            print("Presupuesto Óptimo para la Universidad UAX:")
            print(f"Google Ads: ${optimal_google_budget:.2f}")
            print(f"Facebook Ads: ${optimal_facebook_budget:.2f}")
            print(f"Twitter Ads: ${optimal_twitter_budget:.2f}")
            print(f"ROI Óptimo: {optimal_roi:.2f}")
        else:
            print("No se pudo encontrar una solución óptima para la Universidad UAX.")


# OUTPUT

Presupuesto Óptimo para la Universidad UAX:

Google Ads: $43339.19

Facebook Ads: $30567.40

Twitter Ads: $26093.

ROI Óptimo: 14283600.87
