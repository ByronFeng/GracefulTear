## DNIC��ע�ᡢ��¼����Ȩ����

* .NET CORE SDK >= 2.0
* Visual Studio 2017 Community or JetBrains Rider 2017.3

## ����˵��

* ���� DNIC.AccountCenter Ϊ������Ŀ
* �� Package Manager Console ��������������

		add-migration InitApplicationDbMigration -c ApplicationDbContext
		add-migration InitID4PersistedGrantDbMigration -c PersistedGrantDbContext
		add-migration InitID4ConfigurationDbMigration -c ConfigurationDbContext

* ִ�гɹ���������

		update-database -Context ApplicationDbContext
		update-database -Context PersistedGrantDbContext
		update-database -Context ConfigurationDbContext