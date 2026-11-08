import 'package:flutter/material.dart';

void main() => runApp(const TradingBotApp());

class TradingBotApp extends StatelessWidget {
  const TradingBotApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'AI Trading Bot',
      debugShowCheckedModeBanner: false,
      theme: ThemeData.dark().copyWith(
        scaffoldBackgroundColor: const Color(0xFF0A0E17),
        primaryColor: const Color(0xFF00E676),
      ),
      home: const PlatformSelectScreen(),
    );
  }
}

class PlatformSelectScreen extends StatelessWidget {
  const PlatformSelectScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Padding(
        padding: const EdgeInsets.all(24.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Icon(Icons.smart_toy, size: 72, color: Color(0xFF00E676)),
            const SizedBox(height: 16),
            const Text('AI TRADING BOT', style: TextStyle(fontSize: 26, fontWeight: FontWeight.bold, letterSpacing: 1.5)),
            const Text('Real-Time Market Analysis & Execution', style: TextStyle(color: Colors.grey)),
            const SizedBox(height: 48),
            const Text('SELECT TRADING PLATFORM', style: TextStyle(fontSize: 12, color: Colors.grey, letterSpacing: 1.2)),
            const SizedBox(height: 16),
            Row(
              children: [
                _platformCard(context, 'MetaTrader 5', 'MT5', const Color(0xFF00E676)),
                const SizedBox(width: 16),
                _platformCard(context, 'MetaTrader 4', 'MT4', Colors.amber),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _platformCard(BuildContext context, String title, String code, Color borderColor) {
    return Expanded(
      child: GestureDetector(
        onTap: () => Navigator.push(context, MaterialPageRoute(builder: (_) => BrokerSelectScreen(platform: code))),
        child: Container(
          padding: const EdgeInsets.symmetric(vertical: 32),
          decoration: BoxDecoration(
            color: const Color(0xFF141B29),
            borderRadius: BorderRadius.circular(16),
            border: Border.all(color: borderColor.withOpacity(0.6), width: 1.5),
          ),
          child: Column(
            children: [
              CircleAvatar(backgroundColor: borderColor.withOpacity(0.15), child: Text(code, style: TextStyle(color: borderColor, fontWeight: FontWeight.bold))),
              const SizedBox(height: 12),
              Text(title, style: const TextStyle(fontWeight: FontWeight.bold)),
            ],
          ),
        ),
      ),
    );
  }
}

class BrokerSelectScreen extends StatefulWidget {
  final String platform;
  const BrokerSelectScreen({super.key, required this.platform});

  @override
  State<BrokerSelectScreen> createState() => _BrokerSelectScreenState();
}

class _BrokerSelectScreenState extends State<BrokerSelectScreen> {
  final List<String> allBrokers = ['Exness', 'Deriv', 'IC Markets', 'VT Markets', 'JustMarkets', 'OANDA', 'HFM', 'XM Global', 'OctaFX'];
  String query = '';

  @override
  Widget build(BuildContext context) {
    final filtered = allBrokers.where((b) => b.toLowerCase().contains(query.toLowerCase())).toList();
    return Scaffold(
      appBar: AppBar(title: Text('Select Broker (${widget.platform})'), backgroundColor: Colors.transparent, elevation: 0),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              onChanged: (val) => setState(() => query = val),
              decoration: InputDecoration(
                hintText: 'Search brokers...',
                prefixIcon: const Icon(Icons.search),
                filled: true,
                fillColor: const Color(0xFF141B29),
                border: OutlineInputBorder(borderRadius: BorderRadius.circular(12), borderSide: BorderSide.none),
              ),
            ),
            const SizedBox(height: 16),
            Expanded(
              child: ListView.separated(
                itemCount: filtered.length,
                separatorBuilder: (_, __) => const SizedBox(height: 10),
                itemBuilder: (context, index) {
                  return ListTile(
                    tileColor: const Color(0xFF141B29),
                    shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
                    title: Text(filtered[index], style: const TextStyle(fontWeight: FontWeight.w600)),
                    trailing: const Icon(Icons.chevron_right, color: Colors.grey),
                    onTap: () => Navigator.push(
                      context,
                      MaterialPageRoute(builder: (_) => LicenseAuthScreen(platform: widget.platform, broker: filtered[index])),
                    ),
                  );
                },
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class LicenseAuthScreen extends StatefulWidget {
  final String platform;
  final String broker;
  const LicenseAuthScreen({super.key, required this.platform, required this.broker});

  @override
  State<LicenseAuthScreen> createState() => _LicenseAuthScreenState();
}

class _LicenseAuthScreenState extends State<LicenseAuthScreen> {
  final _keyController = TextEditingController();
  final _accountController = TextEditingController();
  final _passwordController = TextEditingController();
  final _serverController = TextEditingController();
  bool isLoading = false;

  void _authorize() async {
    if (_keyController.text.isEmpty || _accountController.text.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(const SnackBar(content: Text('Please enter your License Key and Account ID')));
      return;
    }
    setState(() => isLoading = true);
    await Future.delayed(const Duration(seconds: 1));
    setState(() => isLoading = false);

    if (!mounted) return;
    Navigator.pushReplacement(
      context,
      MaterialPageRoute(builder: (_) => DashboardScreen(broker: widget.broker, accountId: _accountController.text)),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Broker Authorization'), backgroundColor: Colors.transparent, elevation: 0),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(24.0),
        child: Column(
          children: [
            const CircleAvatar(radius: 32, backgroundColor: Color(0x2200E676), child: Icon(Icons.verified_user, color: Color(0xFF00E676), size: 32)),
            const SizedBox(height: 16),
            const Text('Broker Authorization Key', style: TextStyle(fontSize: 22, fontWeight: FontWeight.bold)),
            Text('Connect to ${widget.broker} (${widget.platform}) servers', style: const TextStyle(color: Colors.grey)),
            const SizedBox(height: 28),
            _inputField(_keyController, 'License Key (e.g. PRO-8821-XP)', Icons.vpn_key),
            const SizedBox(height: 12),
            _inputField(_accountController, 'MT4/MT5 Account ID', Icons.person),
            const SizedBox(height: 12),
            _inputField(_passwordController, 'Trading Password', Icons.lock, obscure: true),
            const SizedBox(height: 12),
            _inputField(_serverController, 'Server (e.g. Exness-MT5Real7)', Icons.dns),
            const SizedBox(height: 24),
            SizedBox(
              width: double.infinity,
              height: 52,
              child: ElevatedButton(
                style: ElevatedButton.styleFrom(backgroundColor: const Color(0xFF00E676), foregroundColor: Colors.black, shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12))),
                onPressed: isLoading ? null : _authorize,
                child: isLoading
                    ? const CircularProgressIndicator(color: Colors.black)
                    : const Text('AUTHORIZE & PROCEED', style: TextStyle(fontWeight: FontWeight.bold, fontSize: 16)),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _inputField(TextEditingController ctrl, String hint, IconData icon, {bool obscure = false}) {
    return TextField(
      controller: ctrl,
      obscureText: obscure,
      decoration: InputDecoration(
        hintText: hint,
        prefixIcon: Icon(icon, color: Colors.grey),
        filled: true,
        fillColor: const Color(0xFF141B29),
        border: OutlineInputBorder(borderRadius: BorderRadius.circular(12), borderSide: BorderSide.none),
      ),
    );
  }
}

class DashboardScreen extends StatefulWidget {
  final String broker;
  final String accountId;
  const DashboardScreen({super.key, required this.broker, required this.accountId});

  @override
  State<DashboardScreen> createState() => _DashboardScreenState();
}

class _DashboardScreenState extends State<DashboardScreen> {
  bool botActive = false;
  double lotSize = 0.01;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('AI Bot Terminal'), backgroundColor: Colors.transparent, elevation: 0),
      body: Padding(
        padding: const EdgeInsets.all(24.0),
        child: Column(
          children: [
            Container(
              padding: const EdgeInsets.all(20),
              decoration: BoxDecoration(color: const Color(0xFF141B29), borderRadius: BorderRadius.circular(16)),
              child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(widget.broker, style: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
                      Text('Account: ${widget.accountId}', style: const TextStyle(color: Colors.grey)),
                    ],
                  ),
