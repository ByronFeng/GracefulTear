## DNIC��ע�ᡢ��¼����Ȩ����

* .NET CORE SDK >= 2.0
* Visual Studio 2017 Community or JetBrains Rider 2017.3

## ��������ע�ᡢ��¼����ӵ��IdentityServer��Ȩ���ܵ�ASP.NET CORE��Ŀ

* ����һ�� Asp.net core ��Ŀ, ѡ���û���Ȩģʽ
* �� Nuget �����������: IdentityServer4.EntityFramework, IdentityServer4, IdentityServer4.AspNetIdentity ������
* �޸� public void ConfigureServices(IServiceCollection services) �����е��������£�

		public void ConfigureServices(IServiceCollection services)
		{
			var connectionString = Configuration.GetConnectionString("DefaultConnection");
			var migrationsAssembly = typeof(Startup).GetTypeInfo().Assembly.GetName().Name;

			services.AddDbContext<ApplicationDbContext>(options => options.UseSqlServer(connectionString));

			services.AddIdentity<ApplicationUser, IdentityRole>()
				.AddEntityFrameworkStores<ApplicationDbContext>()
				.AddDefaultTokenProviders();

			services.AddIdentityServer()
				.AddDeveloperSigningCredential()
				.AddConfigurationStore(options =>
					options.ConfigureDbContext = builder => builder.UseSqlServer(connectionString, sql => sql.MigrationsAssembly(migrationsAssembly))
				)
				.AddOperationalStore(options =>
				{
					options.ConfigureDbContext = builder => builder.UseSqlServer(connectionString, sql => sql.MigrationsAssembly(migrationsAssembly));
					options.EnableTokenCleanup = true;
					options.TokenCleanupInterval = 30;
				})
				.AddAspNetIdentity<ApplicationUser>();


			// Add application services.
			services.AddTransient<IEmailSender, EmailSender>();

			services.AddMvc();
		}
 ע��AddIdentityServerһ��Ҫ��AddIdentity����

* ����Ŀ�����ļ���, ��������EFǨ������

		dotnet ef migrations add InitialIdentityServerPersistedGrantDbMigration -c PersistedGrantDbContext -o Data/Migrations/IdentityServer/PersistedGrantDb
		dotnet ef migrations add InitialIdentityServerConfigurationDbMigration -c ConfigurationDbContext -o Data/Migrations/IdentityServer/ConfigurationDb

* ��Nuget Package Manage Console������

		update-database -Context ApplicationDbContext
		update-database -Context PersistedGrantDbContext
		update-database -Context ConfigurationDbContext

* ��Ŀ�������, �������Ҫ, ������Լ������SeedData�ļ���ӳ�ʼ����