- Graphıfy: https://github.com/Graphify-Labs/graphify check it for visual graphics and relationship architechture
- Prompt bank: https://github.com/f/prompts.chat  check it for if can create a useful thing with it
- Harness Engineering: https://github.com/walkinglabs/learn-harness-engineering  and https://github.com/earendil-works/pi
- To help people about: https://github.com/multica-ai/andrej-karpathy-skills/
- Research topic: use fibonacci sequence in ai programming somehow. look for different alternatives


. Claude Code ve Codex kullananların ne kadarı biliyor bilmiyorum ama bence çok fazla konuşulmayan bir özellik var. Ana agent’ın kendi alt agent’larını açabilmesi, o alt agent’ların da kendi alt agent’larını oluşturabilmesi.
Yani aslında tek bir oturum içerisinde, kendi aralarında iş bölümü yapan ve paralel çalışan koca bir agent ekibi kurabiliyorsunuz. Üstelik bu ekibin tamamının aynı modeli kullanması da gerekmiyor.
Mesela Claude Code üzerinden düşünelim.
Ana agent olarak Opus’u kullanıyorsunuz. Opus’a projenin mimarisini, planlamasını ve orkestrasyonunu bırakıyorsunuz. Opus, işleri Sonnet’lere dağıtıyor. Sonnet’ler kendi aralarında görevleri bölüyor ve daha basit işler için Haiku’ları çalıştırıyor.
İşin güzel tarafı, bu yapı sadece bir ana agent ve onun altında çalışan birkaç agent’tan ibaret değil. İç içe katmanlar oluşturabiliyorsunuz.
Claude Code’da varsayılan olarak 3 katman derinliğe inilebiliyor ve aynı anda 20 alt agent çalıştırılabiliyor. Ama bunlar maksimum sınırlar değil.
İsterseniz 10 katman ve 50 eşzamanlı alt agent olacak şekilde de yapılandırabiliyorsunuz.
Bunun için ~/.claude/settings.json dosyasındaki env alanına iki değişken eklemek yeterli:
"CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH": "10",
"CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS": "50"
İlki agent’ların kaç katman derine inebileceğini, ikincisi aynı anda kaç alt agent çalıştırılabileceğini belirliyor.
Burada bir detay daha var. Claude Code’un eski sürümlerinde bir oturum boyunca oluşturulabilecek toplam alt agent sayısı 200 ile sınırlandırılmıştı. 2.1.224 sürümünde bu toplam sınır da kaldırıldı.
Yani artık bir oturum boyunca toplam kaç agent oluşturabileceğinize ilişkin o eski sınır yok. Eşzamanlı çalışma ve derinlik sınırları ise hâlâ geçerli ve yapılandırılabiliyor.
Modelleri de ayrı ayrı seçebiliyorsunuz. CLAUDE_CODE_SUBAGENT_MODEL ile varsayılan alt agent modelini belirleyebilir veya .claude/agents/ altındaki tanımlarda model: sonnet, model: haiku gibi ayarlar kullanabilirsiniz.
Codex tarafında da benzer bir sistem var.
Klasik multi-agent V1’de varsayılan derinlik 1, eşzamanlı agent sınırı 6. Yani normal ayarlarda alt agent’lar kendi alt agent’larını oluşturmuyor.
Ancak ~/.codex/config.toml dosyasında [agents] bölümüne max_depth = 10 ve max_concurrent_threads_per_session = 50 yazarak bu sınırları değiştirebiliyorsunuz.
Codex’in yeni multi-agent V2 yapısında ise durum biraz farklı. Varsayılan eşzamanlı sınır, ana agent dahil toplam 4. Bunu [features.multi_agent_v2] altındaki max_concurrent_threads_per_session ile artırabiliyorsunuz. Ancak V1’deki max_depth ayarı V2’de dikkate alınmıyor. Dolayısıyla iki sistemin derinlik davranışları aynı değil.
Bir de şu var: 50 agent çalıştırılmasına izin vermek, 50 agent’ın gerçekten sorunsuz çalışacağı anlamına gelmiyor. API limitleri, kaynak tüketimi ve modellerin kullanım sınırları hâlâ işin içinde.
Zaten her görevde onlarca agent çalıştırmanın da pek anlamı yok. Gereksiz yere parçalanmış bir iş, tek agent’la yapılacağından daha maliyetli ve karmaşık hâle gelebilir.
Ama büyük bir projede mimariyi planlayan, özellikleri geliştiren, testleri yazan, kodu inceleyen ve hataları araştıran agent’ları birbirinden ayırmak bence oldukça mantıklı.
Özellikle güçlü bir modeli orkestrasyon için kullanıp daha küçük görevleri hafif modellere dağıtma fikrini seviyorum. Hem ana agent’ın context’ini gereksiz yere doldurmuyorsunuz hem de bağımsız işleri paralel yürütebiliyorsunuz.
Bir bakıma tek bir yapay zekâya kod yazdırmak yerine, yapay zekâların kendi aralarında organize olduğu küçük bir yazılım ekibi kuruyorsunuz.
Tabii ekibin kalabalık olması, ortaya çıkan kodun daha iyi olacağını garanti etmiyor. Orkestrasyonun ve görev dağılımının kalitesi burada çok daha önemli.
