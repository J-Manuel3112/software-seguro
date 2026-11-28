# Objetivo 4: Análisis del Ataque de Algoritmo `"none"` en JWT

## Ataque de algoritmo "None"

Esta vulnerabilidad permite a un atacante falsificar cualquier identidad sin necesidad de conocer sus credenciales. Esto pasa porque el Header de un JWT trae el campo "alg", cuando este acepta "alg : none", un atacante puede quitar la firma al token y tomar la identidad de cualquier usuario. El servidor lo acepta porque nunca valida firma real.

---

## Prevención

Para prevenirlo es importante y necesario implementar una verificación de firma rigurosa y aceptar unicamente algoritmos seguros como "HS256" o "RS256". También se recomienda no usar el "None", junto con un tiempo máximo de duración del token.

--- 