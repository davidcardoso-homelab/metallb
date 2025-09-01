# MetalLB - Provisionamento via ArgoCD

Este projeto automatiza a instalação e configuração do [MetalLB](https://metallb.universe.tf/) em clusters Kubernetes, utilizando o [ArgoCD](https://argo-cd.readthedocs.io/) para gerenciamento GitOps.

## Estrutura do Projeto

- [`metalLB/namespace.yaml`](metalLB/namespace.yaml): Manifesto para criação do namespace `metallb-system`.
- [`metalLB/metallb-config.yaml`](metalLB/metallb-config.yaml): Configuração do MetalLB, incluindo pool de IPs e advertisement L2.
- [`metalLB/repository.yaml`](metalLB/repository.yaml): Secret para adicionar este repositório Git ao ArgoCD.
- [`metalLB/application.yaml`](metalLB/application.yaml): Definição do recurso Application do ArgoCD para instalar o MetalLB a partir deste repositório.

## Passos para Instalação

1. **Configure o acesso ao repositório no ArgoCD**

	Edite o arquivo [`metalLB/repository.yaml`](metalLB/repository.yaml) e insira sua chave SSH privada que tem acesso ao GitHub e a URL do seu repositorio. 
	Se não souber como gerar uma chave, consulte a [documentação oficial do GitHub](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh).

	```sh
	kubectl apply -f metalLB/repository.yaml
	```

2. **Configure o Application do ArgoCD**

	No arquivo [`metalLB/application.yaml`](metalLB/application.yaml), ajuste o campo `repoURL` para corresponder a URL do seu repositório criado anteriormente.

	```sh
	kubectl apply -f metalLB/application.yaml
	```

3. **(Opcional) Ajuste o range de IPs do MetalLB**

	No arquivo [`metalLB/metallb-config.yaml`](metalLB/metallb-config.yaml), altere o campo `addresses` para definir o range de IPs que deseja dedicar ao MetalLB.

## Observações

- O manifesto do namespace é mínimo. Para produção, utilize o manifesto oficial completo do MetalLB.
- O ArgoCD irá sincronizar automaticamente as configurações do MetalLB conforme definidas neste repositório.
- Certifique-se de que o ArgoCD tenha permissão para criar namespaces e recursos no cluster.

---

> Para dúvidas ou sugestões, abra uma issue neste repositório.

