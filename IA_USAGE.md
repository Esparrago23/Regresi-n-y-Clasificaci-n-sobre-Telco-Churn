# Declaración de uso de IA

## Herramientas usadas

- ChatGPT / Codex.

## Qué generé con IA

- Manejo de `TotalCharges` faltante y eliminación de `customerID` + (prompt: "podria usar dropna para eliminar los datos de totalcharges que te parece ya que los datos son muy pocos y para customerID uso drop") + Resumen de la ayuda: Usar `dropna` para los registros con `TotalCharges` faltante es una decisión defendible porque son solo 11 filas de 7,043, aproximadamente 0.16% del dataset. La justificación conceptual es que esos faltantes aparecen al convertir `TotalCharges` desde texto a número y corresponden a clientes con `tenure = 0`. Para regresión, como `TotalCharges` es la variable objetivo, eliminar esas filas evita inventar artificialmente el valor que el modelo debe aprender a predecir. La decisión debe explicarse en markdown: se eliminan porque son pocos casos, no aportan historial de cargos acumulados y no afectan de forma relevante el tamaño del dataset. `customerID` debe eliminarse porque es un identificador único, no un atributo predictor. Mantenerlo puede llevar al modelo a memorizar clientes en lugar de aprender patrones generales.
- Revisión de intento para convertir `TotalCharges` y eliminar `customerID` + (prompt: "df['TotalCharges'] = pd.to_numeric(df['TotalCharges'].dropna()) / df = df.drop(\"customerID\", axis=1) que te parece asi? ayudame donde puedo checar como funciona o como hacerlo añade esto al AI_USAGE") + Resumen de la ayuda: La eliminación de `customerID` con `drop` es correcta conceptualmente porque esa columna es un identificador, no un predictor. La conversión de `TotalCharges` necesita ajustar el orden: primero se debe convertir la columna completa a numérica usando manejo de errores para que los espacios en blanco pasen a `NaN`; después se eliminan las filas faltantes considerando específicamente `TotalCharges`. Usar `dropna()` dentro de `pd.to_numeric()` no es ideal porque los espacios en blanco todavía no son `NaN` antes de convertir, y además se pierde claridad sobre qué filas se están eliminando.

## Qué entendí y modifiqué yo

- Entendí que no basta con limpiar los datos: debo justificar la decisión en el notebook.
- Decidí que eliminar los `NaN` de `TotalCharges` es razonable porque representan muy pocos registros y porque el target de regresión no debería imputarse sin una razón fuerte.
- Entendí que `customerID` no describe comportamiento del cliente, contrato, servicios ni cargos, por eso no debe entrar como predictor.

## Qué NO sé explicar de lo que generó la IA

- Pendiente de completar al final, cuando el notebook tenga todos los modelos y métricas terminados.
