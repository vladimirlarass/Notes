L'attaque Golden Ticket permet aux attaquants de falsifier et de signer des TGT en utilisant le hachage du mot de passe du compte krbtgt. Lorsque ces tickets sont présentés à un serveur AD, les informations qu'ils contiennent ne seront pas du tout vérifiées et seront considérées comme valides car elles sont signées avec le hachage du mot de passe du compte krbtgt.
L' attaque Golden Ticket permet aux attaquants de falsifier et de signer des TGT (Ticket Granting Tickets) en utilisant le hachage du mot de passe du compte krbtgt . Lorsque ces tickets sont présentés à un
serveur AD, les informations qu'ils contiennent ne sont pas du tout vérifiées et sont considérées comme valides
en raison de la signature avec le hachage du mot de passe du compte krbtgt . Par exemple, il est possible de signer
un ticket pour un utilisateur qui n'existe pas, comme DoesNotExist s'il s'agit d'un
, le billet dit aussi
administrateur de domaine, et de demander un ticket TGS (Ticket Granting Service) qui lui permet d'accéder à des
machines distantes. Pour des raisons de discrétion, il est presque toujours préférable d'utiliser des utilisateurs qui
existent dans le domaine. Cependant, mettre de fausses informations dans le ticket peut être un excellent moyen de
montrer l'impact et le manque de surveillance d'une organisation sur ces événements.




