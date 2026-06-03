# RGPD Compliance — Persona

## Données collectées
- Username
- Email
- Topics d'intérêt
- Heure d'envoi
- Style de newsletter
- Date de consentement
- is_public : si le profil est visible par les autres utilisateurs (défaut : false)
- accepts_ads : si l'utilisateur accepte les publicités dans sa newsletter (défaut : true)
- feedback : commentaires de l'utilisateur sur sa newsletter

## Base légale
Consentement explicite de l'utilisateur (Art. 6.1.a RGPD)

## Droits des utilisateurs implémentés

### Droit d'accès (Art. 15)
✅ Via commande "show my profile" dans le chatbot
✅ L'utilisateur peut voir toutes ses données stockées

### Droit de rectification (Art. 16)
✅ Via commandes update_email, update_topics, update_send_time, update_style
✅ Via commandes update_visibility, update_ads_preference

### Droit à l'effacement (Art. 17)
✅ Via commande "delete my account" dans le chatbot
✅ Suppression immédiate de toutes les données personnelles

### Droit d'opposition (Art. 21)
✅ Via commande "unsubscribe" dans le chatbot
✅ Via commande "I don't want ads" pour refuser les publicités

### Droit à la portabilité (Art. 20)
⚠️ Partiellement — l'utilisateur peut voir ses données via get_profile

## Consentement
- Demandé explicitement à l'inscription
- Impossible de s'inscrire sans consentement
- L'utilisateur est informé à l'inscription que des publicités peuvent apparaître
- L'utilisateur peut refuser les publicités à tout moment

## Publicités
- L'utilisateur consent explicitement à recevoir des publicités à l'inscription
- Sans consentement → pas de compte créé
- Les annonceurs ne voient jamais les données personnelles des utilisateurs

## Visibilité du profil
- Par défaut le profil est privé (is_public = false)
- L'utilisateur contrôle entièrement ce qui est visible
- Seuls username, topics et newsletter_style sont visibles en mode public
- Email, send_time et données sensibles ne sont jamais partagés

## Conservation des données
- Données supprimées immédiatement sur demande
- Les feedbacks sont conservés pour améliorer le service
- Pas de backup externe

## Partage des données
- Aucun partage avec des tiers
- Données stockées sur Supabase (serveurs EU disponibles)
- Les annonceurs ne voient jamais les données personnelles des utilisateurs